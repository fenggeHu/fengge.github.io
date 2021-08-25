# 安装Kubernetes
1，先安装docker desktop
2，在docker的Preferences里修改resouces，主要把内存调大（6G+），apply
3，在Preferences里切到Kubernetes，勾选Enable Kubernetes后apply，安装Kubernetes
4，安装 kubectl
```shell
brew install kubernetes-cli
```
5，安装 Kubernetes dashboard
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
```
6，使用 kubectl 命令行工具访问 Dashboard
```shell
kubectl proxy
```
7，获取登录token
```shell
kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token
```
8，登录Kubernetes dashboard
```shell
kubectl 会使得 Dashboard 可以通过 http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/ 访问。
```
# 部署 Kuboard
参考：https://www.kuboard.cn/install/install-dashboard.html#%E5%9C%A8%E7%BA%BF%E4%BD%93%E9%AA%8C
部署v2版本
```shell
kubectl apply -f https://kuboard.cn/install-script/kuboard.yaml
kubectl apply -f https://addons.kuboard.cn/metrics-server/0.3.7/metrics-server.yaml
```
## 
*** 获取Token （此处token与上面一样）

## 访问Kuboard
可以通过NodePort、port-forward 两种方式当中的任意一种访问 Kuboard
1，通过NodePort访问
Kuboard Service 使用了 NodePort 的方式暴露服务，NodePort 为 32567；您可以按如下方式访问 Kuboard。
http://任意一个Worker节点的IP地址:32567/
2，通过port-forward访问
```shell
kubectl port-forward service/kuboard 8080:80 -n kube-system
```
在浏览器打开链接 （请使用 kubectl 所在机器的IP地址）
http://localhost:8080

部署v3版本
```shell
kubectl apply -f https://addons.kuboard.cn/kuboard/kuboard-v3.yaml
```
=# 等待 Kuboard v3 就绪
```shell
kubectl get pods -n kuboard
```
访问Kuboard， http://your-node-ip-address:30080   http://localhost:30080
初始用户名和密码 admin/Kuboard123
注意事项见原文，https://www.kuboard.cn/install/v3/install-in-k8s.html#%E5%AE%89%E8%A3%85

# 安装Istio
1，下载istio。 https://github.com/istio/istio/releases/
2，设置PATH。
```shell
vi ~/.bash_profile

export ISTIO_HOME="~/apps/istio-1.11.0-rc.1"
export PATH="$PATH:${ISTIO_HOME}/bin"
```
3，检查运行环境
```shell
istioctl x precheck
```
4，安装istio组件
```shell
istioctl install --set profile=demo -y
```
通过 kubectl 可以查看在 istio-system 的 Namespace 下安装
```shell
kubectl get all -n=istio-system
```
5，其它可选操作
= 将 default Namespace 设置自动注入。
= 设置自动自动注入后，会往 Pod 中增加一个 sidecar 的 container，用于控制这个 Pod 的流量。
```shell
kubectl label namespace default istio-injection=enabled
```
## 查看目前开启自动注入的 Namespace
```shell
kubectl get ns --show-labels=true
```
# 部署Demo应用
部署bookinfo应用--在istio目录下
```shell
kubectl apply -f ${ISTIO_HOME}/samples/bookinfo/platform/kube/bookinfo.yaml
```
## 检查是否部署完成
```shell
kubectl get all
```
## 查看docker镜像
```shell
docker images | grep istio/examples
```
开启外部流量入口
创建 gateway 和 virtualservice 配置
配置文件：samples/bookinfo/networking/bookinfo-gateway.yaml
gateway：只让 service/istio-ingressgateway 中 HTTP 80 端口的流量进来
virtualservice: 将 URI 为 /productpage等页面的流量指向 productpage 服务
部署gateway 和 virtualservice配置
```shell
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```
## 验证配置
```shell
istioctl analyze
```

# 部署istio工具
部署istio下的所有工具
kubectl apply -f ${ISTIO_HOME}/samples/addons
仅部署kiali，参考：https://kiali.io/documentation/latest/quick-start/
=# To install a Kiali Server using the all-in-one yaml shipped with your version of Istio run the following:
kubectl apply -f ${ISTIO_HOME}/samples/addons/kiali.yaml
=# To uninstall:
kubectl delete -f ${ISTIO_HOME}/samples/addons/kiali.yaml --ignore-not-found
查看工具面板
istioctl dashboard kiali
istioctl dashboard jaeger
... ...