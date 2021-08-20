欢迎来到 Lean 的 OpenWrt 源码仓库！
=

[English](./README_EN.md)

如何编译 OpenWrt 固件
-
注意：
-
1. **不**要用 **root** 用户进行编译！！！
2. 默认登陆IP 192.168.1.1 密码 password

编译命令如下:
-
1. 首先装好 Ubuntu x64，推荐 Ubuntu 20.04 LTS (Focal Fossa)

2. 命令行输入 `sudo apt update` ，然后输入
   `
   sudo apt -y install asciidoc antlr3 autoconf automake autopoint binutils build-essential bzip2 curl device-tree-compiler flex g++-multilib gawk gcc-multilib gettext git git-core gperf lib32gcc1 libc6-dev-i386 libelf-dev libglib2.0-dev libncurses5-dev libtool libssl-dev libz-dev msmtp p7zip p7zip-full patch python2.7 python3 qemu-utils rsync subversion swig texinfo uglifyjs unzip upx wget xmlto zlib1g-dev
   `

3. 使用 `git clone https://github.com/1582130940/OpenWrt` 下载源代码，`cd OpenWrt` 进入目录

4. ```bash
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

5. `make -j16 download V=s` 下载 dl 库

6. `make -j16 V=s` （-j 是线程数。第一次编译推荐单线程）即可开始编译固件。

本套代码保证可以编译成功。里面包括了 R21 所有源代码，包括 IPK。

=

二次编译：
```bash
cd lede
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make -j16 download
make -j$(($(nproc) + 1)) V=s
```

如果需要重新配置：
```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make -j$(($(nproc) + 1)) V=s
```

编译完成后输出路径：bin/targets

如果你使用 WSL 或 WSL2 进行编译：
------
由于 WSL 的 PATH 路径中包含带有空格的 Windows 路径，有可能会导致编译失败，请在将 make -j1 V=s 或 make -j$(($(nproc) + 1)) V=s 改为

首次编译：
```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin make -j1 V=s 
```
二次编译：
```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin make -j$(($(nproc) + 1)) V=s
```

特别提示：
------
1. 源代码中绝不含任何后门和可以监控或者劫持你的 HTTPS 的闭源软件，SSL 安全是互联网最后的壁垒。安全干净才是固件应该做到的；
