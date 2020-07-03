---
title: "LeaveIt主题功能优化"
date: 2020-07-03T16:28:52+08:00
draft: false
categories:
 - 
tags:
---

# 一、评论系统

### 准备工作

LeaveIt主题没有提供评论的功能，因此需要自己写，这里我用的是 [Valine](https://valine.js.org/) 第三方评论系统。必须先注册和创建应用，选着**开发版**就好。

 [快速开始 | Valine](https://valine.js.org/quickstart.html) 

应用创建完后，可以去设置里应用KEY看到 APPID、APPKEY 。

## Step1 配置Valine

 在 `config.toml` 中加入以下代码（Valine基本配置） 

```reStructuredText
 # Valine.
 # You can get your appid and appkey from https://leancloud.cn
 # more info please open https://valine.js.org
 [params.valine]
 enable = true
 appId = 'XXXXXXXXXXXXXXXXXXXXXXXXXXX'
 appKey = 'XXXXXXXXXXXXXXXXXXXXXXXXXXX'
 notify = false  # mail notifier , https://github.com/xCss/Valine/wiki
 verify = false # Verification code
 avatar = 'mm'
 placeholder = '说点什么吧...'
 visitor = true
```

 appId和appKey换成自己的，详细的配置信息请阅读 [配置项 | Valine](https://valine.js.org/configuration.html) 



## Step2 配置comments

 在`themes`目录下的 `layouts/partials/` 中创建 `comments.html` 文件 ，内容如下：

```reStructuredText
<!-- valine -->
{{- if .Site.Params.valine.enable -}}
<!-- id 将作为查询条件 -->

<div id="vcomments"></div>
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script src='//unpkg.com/valine/dist/Valine.min.js'></script>

<script type="text/javascript">
  new Valine({
      el: '#vcomments' ,
      appId: '{{ .Site.Params.valine.appId }}',
      appKey: '{{ .Site.Params.valine.appKey }}',
      notify: '{{ .Site.Params.valine.notify }}', 
      verify: '{{ .Site.Params.valine.verify }}', 
      avatar:'{{ .Site.Params.valine.avatar }}', 
      placeholder: '{{ .Site.Params.valine.placeholder }}',
      visitor: '{{ .Site.Params.valine.visitor }}'
  });
</script>
{{- end }}
```



## Step3 引入comments

在`themes`目录下的 `layouts/_default/single.html` 中加入下面的代码，以引入 `comments.html` 

```text
<div class="post-comment">
    <!-- 加入评论功能 -->
    {{ partial "comments.html" . }}
</div>
```







# 二、文章流量统计

1.在`themes`目录下的`layouts`下的`partials`下的`head.html`中引入不蒜子js文件，代码为

```text
<!-- 不蒜子 -->
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

2.在页面添加统计代码 

找到`themes`目录下的`layouts`下的`partials`下的`footer.html`,添加以下代码

```reStructuredText
<span id="busuanzi_container_site_pv">
    本站访问量：<span id="busuanzi_value_site_pv"></span>次
</span>
&nbsp;
<span id="busuanzi_container_site_uv">
    您是本站第 <span id="busuanzi_value_site_uv"></span> 位访问者
</span>
```

 3.找到`themes`目录下的`layouts`下的`_default`下的`single.html`,添加以下代码 

```reStructuredText
<h5 id="wc" style="font-size: 1rem;text-align: center;">{{ .FuzzyWordCount }} Words|Read in about {{ .ReadingTime }} Min|本文总阅读量<span id="busuanzi_value_page_pv"></span>次</h5>
```

添加的内容和位置根据个人喜好选择









# 三、修改高亮

参考了[大佬的文章]( https://blog.hgtweb.com/2019/code-csdn/ )

 修改 `assts/css/_page/post.scss` 中 `code:not([class])` 的样式代码： 

```scss
/* 原来的样式 */
// code:not([class]) {
//     padding: 5px 5px;
//     background: #fff;
//     border: 1px solid #ddd;
//     box-shadow: 1px 1px 0 #fff, 2px 2px 0 #ddd;
//     margin-left: 3px;
//     margin-right: 3px;

//     .dark-theme &:not([class]) {
//         background: transparent;
//         box-shadow: 1px 1px 0 $dark-font-secondary-color, 2px 2px 0 $dark-font-secondary-color;
//     }
// }

/* 修改成自己想要的样式 */
code:not([class]) {
    padding: 2px 4px;
    background: #f9f2f4;
    color: #ef3982;
    border-radius: 2px;
    margin-left: 3px;
    margin-right: 3px;
    
    .dark-theme &:not([class]) {
       background: #2d2d2d;
       color: #e06c75;
    }
}
```

