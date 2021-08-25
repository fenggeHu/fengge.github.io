1，安装基础工具
# vagrant支持多种虚拟化，我选择virtualbox
```shell
brew install virtualbox
brew install vagrant
```
设置vagrant环境变量
VAGRANT_HOME="/your/path"
修改virtural新建虚拟机文件的默认路径
具体操作是打开VirtualBox，在菜单中选择“管理->全局设定”打开面板，修改默认虚拟电脑位置的路径即可

2，下载linux镜像
```shell
wget https://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box
```
3，加载镜像&初始化-Vagrantfile
```shell
vagrant box add /your/path/virtualbox.box --name YourBoxName
vagrant init YourBoxName
```
4，配置Vagrantfile - 一台虚拟机一段配置
```text
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
# 虚拟机k8smaster
config.vm.define "k8smaster" do |vb|
config.vm.provider "virtualbox" do |v|
v.memory = 2048
v.cpus = 2
end
vb.vm.host_name = "k8smaster"
vb.vm.network :private_network, ip: "172.100.100.100"
vb.vm.network :public_network
vb.vm.box = "centos/7"
vb.vm.box_check_update = false
vb.disksize.size = '30GB'
end

 	# 虚拟机k8snode1
config.vm.define "k8snode1" do |vb|
config.vm.provider "virtualbox" do |v|
v.memory = 4096
v.cpus = 4
end
vb.vm.host_name = "k8snode1"
vb.vm.network :private_network, ip: "172.100.100.101"
vb.vm.box = "centos/7"
end

 	# 虚拟机k8snode2
config.vm.define "k8snode2" do |vb|
config.vm.provider "virtualbox" do |v|
v.memory = 4096
v.cpus = 4
end
vb.vm.host_name = "k8snode2"
vb.vm.network :private_network, ip: "172.100.100.102"
vb.vm.box = "centos/7"
end

end
```
5，vagrant常用命令
```shell
vagrant init #初始化配置；

vagrant up #启动虚拟机；

vagrant ssh #登录虚拟机；

vagrant suspend #挂起虚拟机；

vagrant reload #重启虚拟机；

vagrant halt #关闭虚拟机；

vagrant status #查看虚拟机状态；

vagrant destroy #删除虚拟机。
```
6，vagrant配置
网络配置
有三种网络配置，通常使用NAT、桥接，
private_network（自动生成NAT）：多台虚拟机内部互通；
public_network （桥接）：可指定固定IP，不指定就是自动获取

硬盘大小
