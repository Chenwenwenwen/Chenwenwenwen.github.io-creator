---
title: "使用nvm管理多版本Node"
date: 2020-06-16T14:07:01+08:00
categories: ["工具 · 配置"]
tags: ["配置"]
draft: false
---

# 一、下载nvm

#### github上下载[nvm最新版本](https://github.com/coreybutler/nvm-windows/releases)

- nvm-noinstall.zip 是绿色免安装版本，但是使用之前需要配置（推荐）
- nvm-setup.zip：  这是一个安装包，下载之后点击安装，无需配置就可以使用，很方便



# 二、安装nvm

1. nvm-noinstall.zip 下载完成解压在C:\dev\nvm。里面的文件列表如下

   ![nvm1](/images/assets/nvm1.png )

2. 双击install.cmd，直接回车，生成目录settings.txt文件。把这个文件剪切到C:\dev\nvm目录中。

   （不存在则新建，**新建后的txt文件不要修改其文件编码,使用默认的ANSI格式，否则无法正确解析配置文件中的内容** ）

   ```text
   root: C:\dev\nvm （根据自己情况配置） root:后面一定要有一个空格
   path: C:\dev\nodejs （根据自己情况配置）path:后面一定要有一个空格
   arch: 64 
   proxy: none 
   node_mirror: http://npm.taobao.org/mirrors/node/ 
   npm_mirror: https://npm.taobao.org/mirrors/npm/
   ```

   ​    **注意: root:和path: 后面一定要有一个空格否则在安装node的时候不会安装到nvm文件夹下**

3. 配置环境变量

   window+r 输入sysdm.cpl  快捷方式打开系统属性面板，在高级里面找到环境变量，或者在我的电脑右击属性——》高级系统设置——》高级-——》环境变量

   ```text
   NVM_HOME：nvm.exe 所在目录<==> setting**s**.txt中的root值
   
   NVM_SYMLINK：node 快捷方式所在的目录 <==> setting**s**.txt中的root值
   
   PATH中添加%NVM_HOME% 和**;**%NVM_SYMLINK%
   ```

    验证：打开CMD通过`set NVM_HOME`和 `set NVM_SYMLINK` 命令查看环境变量是否配置成功

   成功则显示如下：

   ![nvm2]( /images/assets/nvm2.png )

4. 在cmd输入nvm 查看nvm详细信息，如果出现详细信息，所以已经安装成功，但是在window10 可能会出现
   ‘nvm’ 不是内部或外部命令，也不是可运行的程序或批处理文件。这是cmd的问题，我们可以打开window+r输入powershell打开powershell，powershell可以看作是cmd 的升级版
   ![nvm3]( /images/assets/nvm3.png )



# 切换node 版本

```text
nvm list查看版本 

nvm use 版本号   （切换到node 版本）

node -v 查看版本   （确认安装成功）
```



# 在vsCode允许node 环境

1.在用户变量里path添加一条环境变量：C:\Users \用户名\AppData\Roaming\npm;

2.在vsCode 扩展中搜索Terminal安装

3.重启vsCode，用terminal 打开控制台即可





### npm的安装

1. 输入 `npm config set prefix “C:\dev\nvm\npm”` 回车，这是在配置npm的全局安装路径，然后在用户文件夹下会生成一个.npmrc的文件，用记事本打开后可以看到   `prefix=C:\dev\nvm\npm`

2. 然后继续在命令中输入： `npm install npm -g` 回车后会发现正在下载npm包，在C:\dev\nvm\npm目录中可以看到下载中的文件，以后我们只要用npm安装包的时候加上 -g 就可以把包安装在我们刚刚配置的全局路径下了。

3. 我们为这个npm配置环境变量： 变量名为：NPM_HOME，变量值为 ：`C:\dev\nvm\npm`

4. 在Path的最前面添加`%NPM_HOME%`，注意了，这个一定要添加在 `%NVM_SYMLINK%`之前，所以我们直接把它放到Path的最前面

5. 输入`npm -v` 检查是否安装成功




同样的我们还可以安装cnpm工具，它是中国版的[npm镜像库](https://cnpmjs.org/)，也是npm官方的一个拷贝，因为我们和外界有一堵墙隔着，所以用这个国内的比较快，[淘宝也弄了一个和npm一样的镜像库](http://npm.taobao.org/)，它和官方的npm每隔10分钟同步一次。安装方式：

- `npm install -g cnpm –registry=http://r.cnpmjs.org`
- 或者用淘宝的`npm install -g cnpm –registry=https://registry.npm.taoba.org`
- 安装好了cnpm后，直接执行`cnpm install` 包名比如：`cnpm install bower -g` 就可以了。-g只是为了把包安装在全局路径下。如果不全局安装，也可以在当前目录中安装，不用-g就可以了。

