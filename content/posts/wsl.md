---
title: "在非系统盘安装 WSL"
date: 2020-06-18T10:51:03+08:00
categories: ["工具 · 配置"]
tags: ["配置"]
draft: false
---

### 在非系统盘安装 WSL

首先在系统中配置

* **windows设置 -- 系统 --  储存 -- 更改新内容的保存位置 -- 新的应用将保持到：非系统盘**

然后以管理员身份运行 Windows PowerShell ， 推荐安装 Windows Terminal (Preview) 更加美观

```cmd
#启动虚拟机平台
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform

#启用Linux子系统
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

#创建目录
New-Item D:\WSL -ItemType Directory
Set-Location D:\WSL

#下载安装程序，这个过程比较慢，要多等一段时间
//直接下载安装包更快
Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1604 -OutFile Ubuntu.appx -UseBasicParsing
Rename-Item .\Ubuntu.appx Ubuntu1604.zip
Expand-Archive .\Ubuntu1804.zip -Verbose

#运行程序
cd Ubuntu1804
.\ubuntu1804.exe
```

检查是否有打开一下Windows功能

![wsl1](/images/assets/wsl1.jpg)



按照上面的步骤完成，就可以轻松吃上 WSL 了 ，不过如果安装过程出错呢？看看下面如何重装吧

### 如何重装

* 直接把 WSL 整个目录删除

* 通过 `wslconfig /l` 查看版本
* 先注销 `wslconfig /u Ubuntu版本`
* 重复之前的步骤重新安装即可

### 更改源

Ubuntu 默认的 apt 源是国外的源，实在太慢了，推荐换成阿里云的源

* **首先复制源文件，便于以后回复**

  ```bash
  sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
  ```

* **查看版本信息**

  ```bash
  lsb_release -c
  // Ubuntu 18.04 的代号是：bionic
  ```

* **编辑源文件**

  ```bash
  sudo vim /etc/apt/sources.list
  ```

* **根据 Ubuntu 版本号，添加相应内容，并把非 aliyun 的资源注释掉**

  ```reStructuredText
  deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
  ```

  保存并退出

* **更新和升级**

  ```bash
  sudo apt-get update
  sudo apt-get upgrade
  ```



### 美化一下 WSL

* **安装 finish shell**

  ```bash
  sudo apt-add-repository ppa:fish-shell/release-3
  sudo apt-get update
  sudo apt-get install fish
  ```

* **安装 oh my fish**

  ```bash
  curl -L https://get.oh-my.fish | fish
  ```

​        需要翻墙：（有办法解决 conection refused 443 ，可以装zsh 来美化终端，和oh my fish 是类似的）

​		 [参考资料1](https://www.cnblogs.com/Dylansuns/p/12309847.html) 

​		 [参考资料2](https://zhuanlan.zhihu.com/p/68336685)

### 安装 nodejs 和 npm

```bash
sudo  apt-get  install  nodejs-legacy
sudo  apt-get  install  npm
```



### 更新 nodejs 和 npm

```bash
npm config set registry http://registry.npm.taobao.org/
sudo npm  install -g  n
sudo n stable
sudo npm install npm@latest -g
```



### 配置 GitLab 的 gitkey

```bash
 // 生成密钥
 ssh-keygen -o -t rsa -b 4096 -C "xxxx@yeezon.com"
 cd .shh
 vim config
 
 // 配置 config 
 ### config
 host code.yeezon.com
 port 59898
 ###
 
 // 设置只有所有者有读和写的权限
 chmod 600 config
 
 // 验证密钥
 ssh -T git@code.yeezon.com
```

当看到 `Welcome to GitLab，@yourname` 的时候就大功告成了



### 如何吃上 WSL 2

参考[链接]( https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-install)

首先需要把 windows 的操作系统版本更新到 **18917** 或以上

**把系统更新到最新版，你需要 windows 易升**

[参考资料](https://www.microsoft.com/en-us/software-download/windows10)

更新完成后，在 windows 更新中检查更新，完成后

加入 **windows 预览体验计划**，选择慢速或快速

通过 `wsl -l` 查看当前的版本

运行 `wsl --set-version 版本 2`



如果你想使 WSL 2 成为默认架构，可以使用以下命令执行此操作

```bash
wsl --set-default-version 2
```

**完成验证发行版使用的 WSL 版本**

```bash
wsl --list --verbose
或
wsl -l -v
```

你在上面选择的发行版现在应该在“version”列下显示“2”。 

现在完成了，你随时可以开始使用你的 WSL 2 发行版了！