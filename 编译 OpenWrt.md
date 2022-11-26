## 注意

1. **不要用 root 用户进行编译**
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1 密码 password

## 编译命令

1. 首先装好 Linux 系统，推荐 Debian 11 或 Ubuntu LTS

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 aria2 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
   libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
   mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip libpython3-dev qemu-utils \
   rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
   ```

3. 下载源代码，更新 feeds 并选择配置

   ```bash
   git clone --depth 1 https://github.com/coolsnowwolf/lede openwrt
   cd openwrt
   sed -i '$a src-git passwall_packages https://github.com/xiaorouji/openwrt-passwall' feeds.conf.default
   sed -i '$a src-git passwall_luci https://github.com/xiaorouji/openwrt-passwall;luci' feeds.conf.default
   sed -i '$a src-git passwall2_luci https://github.com/xiaorouji/openwrt-passwall2' feeds.conf.default
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   wget -O .config https://github.com/iiiiiumd/openwrt/raw/main/.config
   make menuconfig
   ```
   ### OR
   ```bash
   git clone --depth 1 https://github.com/coolsnowwolf/lede openwrt
   cd openwrt
   ./scripts/feeds update -a
   rm -rf feeds/luci/themes/luci-theme-argon
   ./scripts/feeds install -a
   svn export https://github.com/jerrykuku/luci-theme-argon/branches/18.06 package/luci-theme-argon
   svn export https://github.com/fw876/helloworld/trunk package/helloworld
   svn export https://github.com/vernesong/OpenClash/trunk/luci-app-openclash package/luci-app-openclash
   #svn export https://github.com/ophub/luci-app-amlogic/trunk/luci-app-amlogic package/luci-app-amlogic
   #svn export https://github.com/xiaorouji/openwrt-passwall/trunk package/openwrt-passwall
   #svn export https://github.com/xiaorouji/openwrt-passwall/branches/luci/luci-app-passwall package/luci-app-passwall
   #svn export https://github.com/xiaorouji/openwrt-passwall2/trunk/luci-app-passwall2 package/luci-app-passwall2
   svn export https://github.com/iiiiiumd/openwrt/trunk/.config
   make menuconfig
   ```

4. 下载 dl 库，编译固件
（-j 后面是线程数，第一次编译推荐用单线程）

   ```bash
   time make download -j8
   time make V=s -j$(nproc)
   ```

二次编译：

```bash
cd lede
git pull
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
make download -j8
make V=s -j$(nproc)
```

如果需要重新配置：

```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make V=s -j$(nproc)
```

编译完成后输出路径：bin/targets
