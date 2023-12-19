# 常用组件
https://github.com/fenggeHu/some-docker-compose-yml

# 创建子网
```shell
docker network create --subnet=172.25.0.0/16  zookeepernet
## 或者指定driver
docker network create --driver bridge --subnet 172.25.0.0/16 --gateway 172.25.0.1 zookeepernet
```
// 在使用local network时，可能需要绑定hosts

# 启动
```shell
docker-compose up -d
docker-compose -f docker-compose.yml up -d
docker-compose down
```
