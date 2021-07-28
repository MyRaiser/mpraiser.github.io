# OpenWrt入门

记录了OpenWrt的入门步骤和踩的一些坑。

[TOC]

## Helloworld[^helloworld]

### 构建编译环境

[^helloworld]: https://openwrt.org/zh/docs/guide-developer/quickstart-build-images

操作系统：Ubuntu 20.04 LTS
开发板：JS9331开发板

可以先更换国内源。

安装依赖：

```
sudo apt-get install subversion g++ zlib1g-dev build-essential git python rsync man-db
sudo apt-get install libncurses5-dev gawk gettext unzip file libssl-dev wget zip time
```

### 获取OpenWrt的源代码

```bash
git clone https://git.openwrt.org/openwrt/openwrt.git/
cd openwrt
```

 - 如果无法从地址`git clone`，可以访问镜像：

```bash
git clone https://github.com/openwrt/openwrt.git
```

切换到指定的版本，首先查看可用的版本号：

```bash
git tag
```

切换到`v17.01.7`

```bash
git checkout v17.01.7
```

### 更新和安装`feeds`

切换到`./scripts`文件夹下：

```
cd scripts
./feeds update -a
./feeds install -a
```

> `feeds install`使得下载的包在`make menuconfig`中可用。

- 在`./feeds update -a`时遇到端口443拒接访问：

```bash
fatal: 无法访问 'https://git.lede-project.org/feed/packages.git/'：
Failed to connect to git.lede-project.org port 443: 拒绝连接
```

可以在根目录的`feeds.conf.default`中把所有的`https://`改为`git://`。

### 设置编译选项

```bash
make menuconfig
```

#### 安装LuCI
如果上一步`./feeds install -a`正确，那`menuconfig`里应该会多出`LuCI`的选项。

在`LuCI -> 1. Collections`里勾选：

- luci
- luci-ssl-openssl

要添加中文支持，在`LuCI -> 2. Modules -> Translation`里勾选：

- Chinese

> 发现这么设置后无论是否勾选`3. Applications -> luci-app-uhttpd`最终生成的固件中都会包含`httpd`

### 编译

```bash
make
```

如果报错了，使用

```bash
make -j1 V=s
```

查看具体报错信息。

生成的`.bin`文件位于`.bin/targets/`中，对应`menuconfig`里面选择的target。

如果出现了各种：

```bash
check_data_file_clashes: Package libustream-openssl20150806 wants to install file /home/b/Desktop/test_openwrt/openwrt/build_dir/target-i386_pentium4_musl/root-x86/lib/libustream-ssl.so
But that file is already provided by package * libustream-mbedtls20150806
```

可以检查`menuconfig`的选项，往往是配置有问题，实在不行只能删掉配置、清空文件然后重新编译。由于第一次编译时间实在是太长了，我们不喜欢这么干。

```
mv .config .config.bak
make clean
```

### 烧写
#### 使用u-boot + TFTP烧写


### 已知问题

`OpenWrt 19.07.7`编译会报警告

```bash
WARNING: Makefile 'package/feeds/packages/ksmbd/Makefile' has a dependency on 'kmod-crypto-arc4', which does not exist
```

但是实际不影响编译和使用，原因未知。




## OpenWrt目录结构
![](openwrt/dir_structure.png)

如图，第一行是原始目录（由`Git`管理的 + 下载的），第二行是编译生成的目录，作用分别是：

目录名 | 作用
:-: | :-:
tools | 编译时需要一些工具， tools里包含了获取和编译这些工具的命令。里面是一些Makefile，有的可能还有patch。每个Makefile里都有一句 `$(eval $(call HostBuild))`，表示编译这个工具是为了在主机上使用的。
toolchain | 包含一些命令去获取kernel headers, C library, bin-utils, compiler, debugger
target | 各平台在这个目录里定义了firmware和kernel的编译过程。
package | 包含针对各个软件包的Makefile。openwrt定义了一套Makefile模板，各软件参照这个模板定义了自己的信息，如软件包的版本、下载地址、编译方式、安装地址等。
include | openwrt的Makefile都存放在这里。
scripts | 一些perl脚本，用于软件包管理。
dl | 软件包下载后都放到这个目录里
build_dir | 软件包都解压到build_dir/里，然后在此编译
staging_dir | 最终安装目录。tools, toolchain被安装到这里，rootfs也会放到这里。
feeds |
bin | 编译完成之后，firmware和各ipk会放到此目录下。


# LuCI

Lua Configuration Interface

## 结构

### 仓库目录结构

目录名 | 作用
:-: | :-:
applications | 针对其他模块或者应用的单独的一个应用或者插件，可理解为功能模块
i18n | 国际化
modules | 应用集 ？包含主要的用户文件、核心文件
theme | 主题
build | 编译过程的脚本
host | 编译产生的：将语言打包为二进制格式
contrib | /
ipkg-ramips_24kec | 编译产生的：封装make menuconfig阶段选中的页面和功能
libs | 独立的库函数：控件、接口、cbi.js、
po | 语言文件

`modules`下的`luci-base`和`luci-mod-admin-full`包含了网页的基本功能。

### module目录结构

文件/目录名 | 子目录名 | 作用
:-: | :-: | :-:
htdocs | / | html+docs，此目录存放HTML相关文件，主要包含以下两个目录，当烧录到硬件设备后，将拷贝到/www根目录下
/ | cgi-bin | 存放luci启动脚本
/ | luci-static | 存放HTML相关文件，包含CSS、JS及网页图片等文件
luasrc | / | lua+src，此目录存放系统功能的LUA文件及M（model）、V（view）、C（controller）文件夹，当烧录到硬件设备后，将拷贝到/usr/lib/lua/luci目录下
/ | controller | 控制器，生成页面的菜单栏并定义各个页面的调用方法
/ | model | 数据模型，根据底层UCI配置文件生成页面
/ | view | 视图，HTML页面
po | / | 定义页面的语言风格
root | / | 存放配置文件，该目录下的所有文件将拷贝到硬件设备根目录下
src | / | 生成所需要的库文件及LUA脚本
Makefile | / | 定义模块的编译方法

### 板上目录结构

1. `/www`下

文件/目录名 | 作用
:-: | :-:
cgi-bin | 此文件从luci-base下拷贝过来的，存放luci启动脚本
index.html | http请求的主页面，默认是/www/index.html，这个文件里显示了登录时常看见的那句话“LuCI - Lua Configuration Interface”，同时也指定了href链接/cgi-bin/luci
luci-static | 存放HTML相关文件，包含CSS、JS及网页图片等文件。不同主题的htdocs/luci-static都将拷贝到这个目录下

2. `/usr/lib/lua`下

顾名思义，存放了与LUA相关的文件，在LUA脚本中，通过require命令引用的脚本及函数，起始路径都是该目录。同时，不同模型及主题的luasrc文件夹都拷贝到/usr/lib/lua/luci目录下，通过/etc/config/luci中的mediaurlbase字段决定当前使用的主题及语言。

