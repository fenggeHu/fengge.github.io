# 安装测试环境
参考官方文档： https://clickhouse.com/docs/zh/getting-started/install/
```shell
sudo yum install yum-utils
sudo rpm --import https://repo.clickhouse.com/CLICKHOUSE-KEY.GPG
sudo yum-config-manager --add-repo https://repo.clickhouse.com/rpm/stable/x86_64
```

# 配置用户名密码
用户配置文件： /etc/clickhouse-server/users.xml   
在users.xml文件里的users节下设置用户名、密码

# 配置外部可访问
配置文件： /etc/clickhouse-server/config.xml
```xml
<!-- Same for hosts without support for IPv6: -->
    <listen_host>0.0.0.0</listen_host>
```

# 重启服务
```shell
systemctl restart clickhouse-server  #或者 service clickhouse-server restart
```
