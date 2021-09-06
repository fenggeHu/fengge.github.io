# 
在ingress-nginx基础上通过ingress对grpc服务进行访问.

# 创建ssl证书
```shell
openssl req -x509 -nodes -days 36500 -newkey rsa:2048 -keyout grpctls.key -out grpctls.crt -subj "/CN=*.jinfeng.hu/O=jinfeng.hu"
```
说明：
grpctls.key: 私钥文件名
grpctls.crt: 公钥文件名
.jinfeng.hu: 使用此公私钥对的域名必须以.jinfeng.hu为后缀

# 创建secret
```shell
kubectl create secret tls grpcs-secret --key grpctls.key --cert grpctls.crt -n jinfeng
```

# node打标
```shell
# 使用DaemonSet方式部署ingress-nginx到集群中指定的node。这里把指定的node打上label
kubectl label node docker-desktop isIngress="true"
```

# 安装ingress-nginx/controller
使用DaemonSet+HostNetwork+nodeSelector方式，所以修改了kind:Deployment为kind:DaemonSet及配置，修改处有注释.
```shell
https://fenggehu.github.io/k8s/ingress-nginx-controller-v1.0.0.yaml
```

# backend 服务参考配置
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: grpc-server
  name: grpc-server
  namespace: jinfeng
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grpc-server
  template:
    metadata:
      labels:
        app: grpc-server
    spec:
      restartPolicy: Always
      containers:
        - env:
            - name: JAVA_OPTS
              value: -Xmx1G -Xms1G
          image: harbor-test.jinfeng.io/example/grpc-server:v2
          imagePullPolicy: IfNotPresent
          name: grpc-server
          ports:
            - containerPort: 50051
              name: grpc
              protocol: TCP
            - containerPort: 5100
              name: http
              protocol: TCP
          # volumeMounts:
          # - mountPath: /jinfeng/log/grpc-server
          #   name: app-logs
        - args:
            - -c
            - /etc/filebeat.yml
            - -e
          env:
            - name: CLUSTER_NAME
              value: EKS
          image: docker.elastic.co/beats/filebeat:6.4.0
          lifecycle:
            preStop:
              exec:
                command:
                  - /bin/sleep
                  - "5"
          name: filebeat
          resources:
            limits:
              cpu: "1"
              memory: 256Mi
            requests:
              cpu: 100m
              memory: 128Mi
          volumeMounts:
            - mountPath: /etc/filebeat.yml
              name: filebeat-config
              readOnly: true
              subPath: filebeat.yml
            - mountPath: /mnt/log
              name: app-logs
      terminationGracePeriodSeconds: 30
      volumes:
        - emptyDir: { }
          name: app-logs
        - configMap:
            defaultMode: 420
            name: grpc-server-filebeat-config
          name: filebeat-config
      imagePullSecrets:
        - name: jinfeng-harbor-hangzhou
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: grpc-server
  name: grpc-server
  namespace: jinfeng
spec:
  type: ClusterIP
  ports:
    - name: grpc
      port: 5051
      # nodePort: 30520    # 集群外可连接的端口，默认随机，范围30000-32767
      protocol: TCP
      targetPort: 50051
    - name: http
      port: 5050
      protocol: TCP
      targetPort: 50050
  selector:
    app: grpc-server
---
apiVersion: v1
data:
  filebeat.yml: |-
    filebeat.inputs:
    - type: log
      processors:
      paths:
        - "/mnt/log/*.log"
      fields:
        app: grpc-server
        group: jinfeng
        namespaces: jinfeng
        clusterName: "${CLUSTER_NAME:}"
      close_inactive: 1m
      close_timeout: 3h
      clean_inactive: 72h
      ignore_older: 70h
      multiline.pattern: ^20[0-9]{2}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}:[0-9]{2}
      multiline.negate: true
      multiline.match: after
      multiline.timeout: 50ms
      multiline.max_lines: 40
      exclude_files: ['debug']
    output.logstash:
      hosts: ["logstash-http.elastic-system:5045"]
kind: ConfigMap
metadata:
  name: grpc-server-filebeat-config
  namespace: jinfeng
```

# Ingress参考配置
gRPC协议单独配置
```yaml
# HTTP/2.0 - gRPC 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/backend-protocol: "GRPC"
    nginx.ingress.kubernetes.io/http2-push-preload: "true"
  name: grpc-server-ingress
  labels:
    app: grpc-server-ingress
    grpc.jinfeng.hu/name: grpc-server-ingress
  namespace: jinfeng
spec:
  rules:
    - host: grpc.jinfeng.hu
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: grpc-server
                port:
                  number: 5051 # 对应service端口
  tls:
    # This secret must exist beforehand
    # The cert must also contain the subj-name grpctest.dev.mydomain.com
    # https://github.com/kubernetes/ingress-nginx/blob/master/docs/examples/PREREQUISITES.md#tls-certificates
    - secretName: grpcs-secret
      hosts:
        - grpc.jinfeng.hu

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    #    nginx.ingress.kubernetes.io/backend-protocol: "GRPC"
    nginx.ingress.kubernetes.io/http2-push-preload: "true"
  name: https-server-ingress
  labels:
    app: https-server-ingress
    grpc.jinfeng.hu/name: https-server-ingress
  namespace: jinfeng
spec:
  rules:
    - host: grpc.jinfeng.hu
      http:
        paths:
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: grpc-server
                port:
                  number: 5050  # 对应service端口
  tls:
    # This secret must exist beforehand
    # The cert must also contain the subj-name grpctest.dev.mydomain.com
    # https://github.com/kubernetes/ingress-nginx/blob/master/docs/examples/PREREQUISITES.md#tls-certificates
    - secretName: grpcs-secret
      hosts:
        - grpc.jinfeng.hu
```
ingress-nginx官方示例，https://github.com/kubernetes/ingress-nginx/tree/main/docs/examples/grpc

# java grpc ssl
1，借助ingress-nginx的能力，服务端不需要改造
2，客户端需要启用TLS，指定sslContext。
客户端参考代码：
```java
// 创建SslContext
private static SslContext sslContext = buildSslContext();
@SneakyThrows
private static SslContext buildSslContext(/**String trustCertCollectionFilePath,
                                          String clientCertChainFilePath,
                                          String clientPrivateKeyFilePath */) {
    SslContextBuilder builder = GrpcSslContexts.forClient();
    File crtFile = ResourceUtils.getFile("classpath:grpctls.crt");
    builder.trustManager(crtFile);
    return builder.build();
}

// 构建 NettyChannelBuilder
NettyChannelBuilder builder = NettyChannelBuilder.forAddress(address).negotiationType(NegotiationType.TLS).sslContext(sslContext);
```
