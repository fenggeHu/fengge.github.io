# 准备编译环境
https://oldwiki.archive.openwrt.org/doc/howto/buildroot.exigence.macosx

- 搞定了编译环境，无论选择哪个版本，后面的流程都一样的

# 选择一个openwrt版本
- openwrt有很多fork版本，选择一个拉取代码到本地
```shell
git clone https://github.com/coolsnowwolf/lede
cd lede
```

# 更新feeds并选择固件
```shell
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```

# 下载 dl 库，编译固件 （-j 后面是线程数，第一次编译推荐用单线程）
```shell
make download -j8
make V=s -j1
```
# 二次编译
```shell
git pull
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
make download -j8
make V=s -j$(nproc)
```