---
title: "利用flex设置了弹性项目如何增大或缩小以适应其弹性容器中可用的空间"
date: 2021-01-05T14:47:51+08:00
categories: ["编程 · 技术"]
tags: ["css","组件"]
draft: false
---

# 利用flex设置了弹性项目如何增大或缩小以适应其弹性容器中可用的空间

原理：利用flex属性，原本每个设置为1，当鼠标hover在上面时，改为flex：4撑大

效果：

![flex效果图](/images/assets/flex效果图.png)

### html代码

```html
<div class="dz_index-gallery">
    <div class="dz_index-gallery-wrapper">
    
     <!-- 重复模块 -->
      <div class="dz_index-gallery-list" style="background-color: skyblue;">
        <div class="caption-cont">
          <h4 class="title">123</h4>
          <p class="intro">test1</p>
        </div>
      </div>
      <!--end-->
      
      <div class="dz_index-gallery-list" style="background-color: yellow;">
        <div class="caption-cont">
          <h4 class="title">231</h4>
          <p class="intro">test2</p>
        </div>
      </div>
      
      <div class="dz_index-gallery-list" style="background-color:pink;">
        <div class="caption-cont">
          <h4 class="title">9999</h4>
          <p class="intro">test3</p>
        </div>
      </div>
      
      <div class="dz_index-gallery-list" style="background-color:green;">
        <div class="caption-cont">
          <h4 class="title">3333</h4>
          <p class="intro">test4</p>
        </div>
      </div>
      
    </div>
  </div>
```

### css代码

```css
<style>
  .dz_index-gallery {
    width: 100%;
    box-sizing: border-box;
    overflow: hidden;

    padding: 30px 0;
  }
  .dz_index-gallery * {
    user-select: none;
    box-sizing: border-box;
  }
  .dz_index-gallery-wrapper {
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
      width: 100%;
      height: 60vh;
      min-height: 300px;
  }
    .dz_index-gallery-list {
      position: relative;
      flex: 1;
      height: 100%;
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center;
      transition: all .5s ease-in-out;
    }
    .dz_index-gallery-list .caption-cont {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      padding: 10%;
      color: #fff;
      background: linear-gradient(to left,rgba(0, 45, 78, 0.7),rgba(0, 45, 78, 0.3));
      opacity: 0;
      transition: opacity 0s 0s;
    }
    .dz_index-gallery-list .caption-cont .title {
      margin: 0 0 20px 0;
      font-size: 20px;
    }
    .dz_index-gallery-list .caption-cont .intro {
      font-size: 16px;
      line-height: 1.5;
    }
    .dz_index-gallery-list:hover {
      flex: 4;
    }
    .dz_index-gallery-list:hover .caption-cont {
      opacity: 1;
      transition: opacity .3s .5s;
    }
  </style>
```

