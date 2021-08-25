安装k8s集群

容器运行时
安装Linux下的容器：https://kubernetes.io/zh/docs/setup/production-environment/container-runtimes/
Docker
Docker版本与K8s版本要匹配，避免兼容问题
在每个节点上，根据安装 Docker 引擎为你的 Linux 发行版安装 Docker。 你可以在此文件中找到最新的经过验证的 Docker 版本依赖关系。
配置 Docker 守护程序，尤其是使用 systemd 来管理容器的 cgroup。
```shell
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "registry-mirrors":["https://registry.docker-cn.com"]
}
EOF
```
重新启动 Docker 并在启动时启用
sudo systemctl enable docker
sudo systemctl daemon-reload
sudo systemctl restart docker

Master启动
```shell
sudo kubeadm init
```
# 指定k8smaster的ip:port， 172.100.100.100:6443
```shell
sudo kubeadm init --control-plane-endpoint "LOAD_BALANCER_DNS:LOAD_BALANCER_PORT" --upload-certs
```
启动成功后打印出如下日志：
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 172.100.100.100:6443 --token f50vvs.33qfhpm9agz8s70l \
	--discovery-token-ca-cert-hash sha256:04cb29f56f122f9a4b4ccce7cc41ddffdac18f78180b495f3e3ec428ef0b0391 \
	--control-plane --certificate-key f9329be0b97463a069681c8f2885dea94df37a368caa83733c84cd96339e5170

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.100.100.100:6443 --token f50vvs.33qfhpm9agz8s70l \
	--discovery-token-ca-cert-hash sha256:04cb29f56f122f9a4b4ccce7cc41ddffdac18f78180b495f3e3ec428ef0b0391


# 拉取k8s镜像需要翻墙，先拉取镜像
```shell
sudo kubeadm config images pull
```