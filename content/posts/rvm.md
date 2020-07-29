---
title: "使用rvm管理多版本Ruby"
date: 2020-07-27T14:16:12+08:00
categories: ["工具 · 配置"]
tags: ["配置"]
draft: false
---

# 使用rvm管理多版本ruby

### 一、安装rvm

1. 如果没有安装 curl，先安装 

   ```ruby
   curl sudo apt-get install curl
   ```

2. 安装 RVM 

   ```ruby
   \curl -sSL [https://get.rvm.io](https://get.rvm.io/) | bash -s stable 
   ```

   下载连接被墙，解决方法：https://www.cnblogs.com/Dylansuns/p/12309847.html

   安装成功：

   ![rvm1](/images/assets/rvm1.png)

3. 加载 RVM，加载后才能使用！！ 

   ```reStructuredText
   source ~/.bashrc
   source ~/.bash_profile
   source ~/.profile
   ```

4. 查看版本

   ```reStructuredText
   rvm -v
   ```

   正常结果：

   ```reStructuredText
    rvm 1.29.9 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io] 
   ```

   

### 二、安装ruby

1. 看下rvm下的所有ruby版本的程序 

   ```reStructuredText
    rvm list known 
   ```

2. 安装你想要的ruby版本

   ```reStructuredText
   rvm install 2.5.3
   ```

3. 如果报以下错误

   `/etc/apt/sources.list` 执行下面，两三次

   ```reStructuredText
   rvm autolibs disable
   ```

4. 使用指定版本 

   ```reStructuredText
   ruby rvm 1.9.2
   ```

5. 查看当前 ruby 版本

   ```text
    ruby -v 
   ```

6. 设置 RVM 默认版本 

   ```reStructuredText
   rvm --default use 1.9.2
   ```

7. 使用默认版本 

   ```reStructuredText
   ruby rvm default 
   ```

8. 删除一个版本 

   ```text
   rvm remove 1.9.2 
   ```

   

