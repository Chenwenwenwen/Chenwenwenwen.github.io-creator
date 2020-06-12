---
title: "如何使用Hugo搭建个人博客"
date: 2020-06-12T11:07:48+08:00
draft: false
---

# Hugo是什么？

 Hugo是一个用Go语言编写的静态网站生成器，Hugo一般只需要几秒钟就能生成一个网站（每页少于1毫秒），被称为”世界上最快的网站构建框架“，是最热门的静态网站生成器之一，被广泛采用。 

 Hugo 官方主页：https://gohugo.io/ 



# 如何使用Hugo搭建个人博客

### Step1：安装Hugo

1.下载

[Hugo安装]: https://github.com/gohugoio/hugo/releases	"Hugo安装"

![1591931792129](C:\Users\chenwenjing\AppData\Roaming\Typora\typora-user-images\1591931792129.png)

2.下载到安装包，解压后放到x:\xxx\hugo

3.添加环境变量：点击计算机图标-右键-属性-高级系统设置-系统变量-path-添加

![1591941569064](C:\Users\chenwenjing\AppData\Roaming\Typora\typora-user-images\1591941569064.png)

4.重启终端，运行 `hugo version` 。安装成功就能查看到版本号

![1591941663996](C:\Users\chenwenjing\AppData\Roaming\Typora\typora-user-images\1591941663996.png)



### Step2：创建一个新的Hugo网页

1.进入

[Hugo官网]: https://gohugo.io/	"Hugo官网"

2.点击Quick Start ，Step1操作前面已完成，直接开始Step2

3.复制Step2的代码，在cmd上运行。注意这里要把”quickstart“改成“github用户名.github.io-creater!”(这样操作的好处是，上传到GitHub上后方便标识。) 

hugo new site quickstart 

上面的代码将在名为的文件夹中创建一个新的Hugo网站 quickstart 



### Step3：添加主题

这里以主题ananke为例：

cd quickstart //记得将quickstart更改为上一步的创建名
git init 
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke 


将主题添加到站点配置中：

echo 'theme = "ananke"' >> config.toml 




### Step4：新建一篇文章

posts:放置文章的文件夹

hugo new posts/test.md 


 可以用文本文件或 Markdown 编辑器打开文件 post/test.md ，并增加点内容。 

![1591942864064](C:\Users\chenwenjing\AppData\Roaming\Typora\typora-user-images\1591942864064.png)

PS：默认创建的是草稿类型，需要将draft值改为false才能看到页面

#### 推送新文章

在网站的主目录下

hexo new posts/文章名。md
hugo --theme=主题名称 
cd public
git add -A
git commit -m "描述内容"
git push -u origin master


### Step5：开启Hugo服务器

hugo server -D 


 **浏览至[http：// localhost：1313 /的](http://localhost:1313/)新站点。** 

 随意编辑或添加新内容，只需在浏览器中刷新即可快速查看更改（您可能需要在Web浏览器中强制刷新，通常使用Ctrl-R之类的功能）。 



### Step6：更换主题

 `config.toml`在文本编辑器中打开：


baseURL = "https://example.org/" 

languageCode = "en-us" //可换为zh-hans

title = "My New Hugo Site" 

theme = "ananke" //主题名




### Step7：生成静态网站


hugo --theme=主题名 --baseUrl="https://YOURNAME.github.io/" 


所有静态页面都会生成到 public 目录下，生成静态网站后并push到你的GitHub Pages 上，就能得到一个在线的个人博客了。

如果一切顺利，所有静态页面都会生成到 public 目录，将 pubilc 目录里所有文件 push 到刚创建的 Repository 的 master 分支。

```
cd public 

git init 

git remote add origin https://github.com/coderzh/coderzh.github.io.git 

git add -A 

git commit -m "first commit" 

git push -u origin master 
```

 在GitHub上新建一个仓库，命名为github用户名.github.io 

1. 把public目录上传到新建的仓库中
2. 在你的git仓库里面Settings的GitHub Pages下你可以看到http://你的用户名.github.io的链接。
3. 打开此链接，就能看到你的Hugo已经被挂到网上了



# 大功告成

## 还可以这样做：

1. 在你的GitHub上新建一个仓库，命名为github用户名.github.io-creator
2. 将Hugo根目录的文件上传到此仓库(注意：Hugo根目录上存储着原代码，Hugo根目录丢失，就无法写博客了)