# [Build system setup](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem)

# 下载解压相应架构的 [OpenWrt SDK](https://downloads.openwrt.org/)

# 进入 OpenWrt SDK 目录
```console
sed -i '$a src-git passwall_packages https://github.com/xiaorouji/openwrt-passwall' feeds.conf.default
sed -i '$a src-git passwall_luci https://github.com/xiaorouji/openwrt-passwall;luci' feeds.conf.default
sed -i '$a src-git passwall2_luci https://github.com/xiaorouji/openwrt-passwall2' feeds.conf.default

./scripts/feeds update -a

# 可选
rm -rf feeds/packages/lang/golang
svn export https://github.com/coolsnowwolf/packages/trunk/lang/golang feeds/packages/lang/golang

./scripts/feeds install -p passwall_packages luci-app-{passwall,passwall2}

svn export https://github.com/unifreq/openwrt_packit/trunk/files/openwrt_config_demo/arm64-r22.11.18-20221124-huge.config .config
make menuconfig

time make download -j8

time make package/luci-app-{passwall,passwall2}/compile V=s
```
