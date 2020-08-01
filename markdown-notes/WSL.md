# WSL使用体验及食用方法

[TOC]

## WSL介绍
> Windows Subsystem for Linux（WSL）是一个在Windows 10上能够运行> 原生Linux二进制可执行文件（ELF格式）的兼容层。它是由微软与> Canonical公司合作开发，其目标是使纯正的Ubuntu 14.04 "Trusty > Tahr"（等Linux发行版的）映像能下载和解压到用户的本地计算机，并且映像内的工具和实用工具能在此子系统上原生运行。

- 相比于虚拟机他的优势有：

    占用内存和CPU资源更少。在WSL上直接运行软件消耗资源和在Windows上差不多。Windows可以直接访问WSL环境。

- 相比于多系统他的优势有 

    不需要重启更换系统（省事），文件交互简单。

## WSL食用方法
一个主要的参考文档：

[微软官方的WSL说明文档](https://docs.microsoft.com/en-us/windows/wsl/install-on-server)

主要步骤如下：

1. 开启Windows中的WSL组件。

    略。

2. 安装WSL的Linux发行版镜像

    通常可以直接在Microsoft Store安装。由于我是LTSC 2019，没有Microsoft Store，非常尴尬。百度了一圈之后找到了直接下载的链接：[手动下载Linux镜像的链接](https://docs.microsoft.com/en-us/windows/wsl/install-manual)，挑选一个使用**迅雷**下载即可。

    下载完成后，后缀名改成.zip解压，找到文件夹中对应Linux发行版的启动器进行安装。（但官方文档确实就是这么写的）

3. 运行WSL

    直接运行Linux发行版的启动器；或者配置好环境变量后，在Powershell/CMD中运行wsl或者bash。

## 给WSL加上GUI
需要说明的是WSL本身不支持GUI，但是可以通过XServer间接实现GUI，使用的软件为[VcXsrv](https://sourceforge.net/projects/vcxsrv/)。这部分主要参考了[这篇文章](https://blog.csdn.net/w_weilan/article/details/82862913)。

大致工作原理为：
> 1. VcXsrv启动Xserver服务用于监听；
> 2. WSL启动程序后把界面数据发送给Xserver；
> 3. Xserver接收到数据进行绘制，于是在Win下看到图形界面。

主要步骤如下：
1. 下载安装和启动[VcXsrv](https://sourceforge.net/projects/vcxsrv/)。一直下一步就行。
   
2. 配置DISPLAY
   
    使用vi打开
    ```bash
    sudo vi ~/.bashrc
    ```

    在最后加上一行：
    ```bash
    export DISPLAY=:0.0
    ```

    保存退出后更新一下：
    ```bash
    source ~/.bashrc
    ```

3. 打开VcXsrv的**XLaunch**，配置看着选就行。

    这个时候XLaunch已经在Windows后台运行了，在WSL打开带GUI的程序时会弹出窗口。比如试一试Firefox：
    ```bash
    sudo apt install firefox
    firefox
    ```
    > 我换了Ubuntu 20.04后firefox变成能打开但是无法正常使用了，不知道为什么。

    还有个问题就是中文显示不全。安装一下字体包即可解决：
    ```bash
    sudo apt install fonts-noto-cjk
    ```

4. 安装桌面
   
    选用的是**xfce4**，使用：
    ```bash
    sudo apt install xfce4 dbus-x11
    ```

    打开XShell并在WSL执行：
    ```bash
    xfce4-session
    ```
    即可进入桌面。

5. 使用vscode
   
    由于相距原教程已经过去一段时间，vscode已经更新了新的特性，原教程中安装vscode的方法不再必要（也不可行，在WSL运行vscode会报错）。现在不需要在WSL中另外安装visual-studio-code，只需要在Windows的vscode中安装Remote - WSL插件，再在WSL中键入：
    ```bash
    code
    ```
    即可。

## 关掉WSL的提示音
WSL有许多毫无必要并且非常刺耳的提示音，听多了特别令人烦躁，因此可以想办法关掉它。

bash
1.sudo vi /etc/inputrc
2.取消注释 set bell-style none
3.退出重启

> 但是这个不能取消掉VIM里的提示声，你可以选择关闭系统声音，按照如下步骤：打开控制面板——>打开硬件和声音——>打开声音——>选择声音——>修改关键性停止的声音方案，来取消提示音。
> 
> ![](https://pic1.zhimg.com/50/07378255d876ce3830964b5f0b29b05a_hd.jpg)
> 
> 链接：https://www.zhihu.com/question/42228124/answer/115022693
> 来源：知乎


## 更换源
Ubuntu自带源在国内访问速度太慢，因此可以更换国内的源，比如清华源、中科大源、[阿里云源](https://developer.aliyun.com/mirror/)等。

1. 先备份一下原来的，以免搞坏了。
    ```bash
    cd /etc/apt
    sudo cp sources.list sources.list.backup
    ```
    复制了一份并命名为`sources.list.backup`，到时候再`cp`回来就行。

2. 更改源配置
    ```bash
    sudo vi sources.list
    ```
    可以把里面原来的都删掉或者注释掉，然后加上网站上复制来的源，下面提供一些[范例](https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.3e221b11waT1Yb)：

    - ubuntu 16.04 配置如下：
        ```bash
        deb http://mirrors.aliyun.com/ubuntu/ xenial main
        deb-src http://mirrors.aliyun.com/ubuntu/ xenial main

        deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
        deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main

        deb http://mirrors.aliyun.com/ubuntu/ xenial universe
        deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
        deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
        deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

        deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
        deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
        deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
        deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
        ```

    - ubuntu 18.04(bionic) 配置如下：
        ```bash
        deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
        ```

    - ubuntu 20.04(focal) 配置如下：
        ```bash
        deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

        deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
        ```

3. 更新

    保存退出之后，更新一下`apt`就可以正常使用了。
    ```bash
    sudo apt-get update
    ```

> 特别注意，源需要换对应OS版本的，不然可能会出现
> ```
> The following packages have unmet dependencies:
> ```
> 这是因为源对应系统版本不对，找不到依赖了。


