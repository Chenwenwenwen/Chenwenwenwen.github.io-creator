---
title: "JQ实现侧边栏手风琴三级导航菜单Demo"
date: 2021-01-05T15:11:16+08:00
categories: ["编程 · 技术"]
tags: ["JQ","组件"]
draft: false
---


效果：

![手风琴导航效果图](/images/assets/手风琴导航效果图效果图.png)

### html代码

```html
<div class="dz_pro_nav">
    <div class="pro_nav">
    <ul class="nav">
        <li>
        <div  class="fristNav">
          <a  href="#" target="_blank" >
            <span class="DXIcon dx-icon_year dz_nav_icon pc"></span>
            <h2>一级菜单</h2>
          </a>
 		  <!--三角箭头-->
          <div class="nav_triangle_1 fristNav_open"></div>
        </div>
          <!-- 二级菜单 -->
          <ul class="sub">
            <li>
                <div class="secoundNav" >
                 <a  href="#" target="_blank" >
                  <div class="sub-title">二级菜单</div>
                 </a>
                 <!--三角箭头-->
                 <div class="nav_triangle_1 secoundNav_open"></div>
                </div>
                <!-- 三级菜单 -->
                <div class="thr">
                    <a class="thridNav" href="#" target="_blank" >
                     <!--三角箭头-->
                    <div class="nav_triangle_2"></div>
                    <div class="thr_title">三级菜单</div>
                  </a>
                </div>
            </li>
          </ul>
        </li>
    </ul>
  </div>
</div>
```



### CSS代码

```css
.dz_pro_nav{
    margin-right: 10px;
    width: 190px;
  }
  .pro_nav *{
    margin: 0;
    padding: 0;
  }
  .pro_nav{
    background-color: #ffffff;
    width: 190px;
    height: auto;
  }
  .pro_nav .nav{
    list-style: none;
    width: 100%;
  }
  .nav>li{
    width: 100%;
    height: auto;
    background-color: #ffffff;
    padding: 25px 0 15px 0;
    border-bottom: 1px solid #e2e2e2;
  }
  .fristNav{
    display: flex;
    justify-content: flex-start;
    align-items: center;
    text-indent:2em;
    position: relative;
    padding-bottom: 10px;
  }
  .fristNav a{
    display: flex;
    justify-content: flex-start;
    align-items: center;
    text-decoration:none；
  }
  .dz_nav_icon{
    color: #002D4E;
    font-size: 18px;
    font-weight: 700;
  }
  .fristNav h2{
    font-size: 18px;
    line-height: 24px;
    letter-spacing: 1px;
    color: #002D4E;
    font-weight: 700;
    text-indent:0.5em;
    cursor: pointer;
  }
  .nav_triangle_1{
    width:0 ;
    height: 0;
    border: 7px solid transparent;
    border-top-color: #D2D2D2;
    border-bottom: none;
    border-left-color: transparent;
    border-right-color: transparent;
    position: absolute;
    right: 30px;
    cursor: pointer;
  }
  .sub{
    display: none;
  }
  .sub>li{
    list-style: none;
    width: 100%;
    height: auto;
    margin: 10px 0;
  }
  .secoundNav{
    text-indent: 4em;
    position: relative;
    display: flex;
    align-items: center;
  }
  .sub-title{
    font-size: 15px;
    line-height: 30px;
    color: #494949;
    font-weight: 400;
    cursor: pointer;
  }
  .sub .sub_active_title{
    color: #1A1A1A;
    font-weight: 700;
  }
  .sub .sub_active{
    text-indent: 3.6em;
    border-left: 6px solid #002D4E;
  }
  .thr{
    display: none;
  }
  .thridNav{
    margin-left: 4em;
    position: relative;
    display: flex;
    align-items: center;

    width: 100%;
    height: 30px;
    padding: 10px 0;
  }
  .thr_title{
    font-size: 14px;
    line-height: 30px;
    color: #929292;
    font-weight: 400;
    margin-left: 10px;
    cursor: pointer;
  }
  .nav_triangle_2{
    display: inline-block;
    vertical-align: middle;
    width:0 ;
    height: 0;
    border: 8px solid transparent;
    border-left-color:#ffffff;
    border-right: none;
    border-top-color: transparent;
    border-bottom-color: transparent;
    margin-right: 10px;
  }
  .nav_triangle_2_active {
    border-left-color:  #002D4E;
  }
  .thr_title_active{
    color: #494949;
  }
```



### JQ代码

```js
 $(function() {
      $('.fristNav_open').click(function() {
        $(this).parent().next().slideToggle();
      });
      $('.secoundNav_open').click(function() {
          $(this).parent().next().slideToggle();
          if ($(this).parent().hasClass('sub_active')) {
            $(this).parent().removeClass('sub_active');
            $(this).parent().find('.sub-title').removeClass('sub_active_title');
          } else {
            $(this).parent().addClass('sub_active');
            $(this).parent().find('.sub-title').addClass('sub_active_title');
          }
      });
      $('.thridNav').click(function() {
          if ($(this).find('.nav_triangle_2').hasClass('nav_triangle_2_active')) {
            $(this).find('.nav_triangle_2').removeClass('nav_triangle_2_active');
            $(this).find('.thr_title').removeClass('thr_title_active');
          } else {
            $(this).find('.nav_triangle_2').addClass('nav_triangle_2_active');
            $(this).find('.thr_title').addClass('thr_title_active');
          }
      });
    })
```


