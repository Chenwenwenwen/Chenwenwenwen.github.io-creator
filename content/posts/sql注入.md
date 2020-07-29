---
title: "Sql注入基础原理"
date: 2020-07-03T10:27:49+08:00
categories: ["编程 · 技术"]
tags: ["基础知识"]
draft: false
---

## 一、Sql注入简介

​	Sql注入攻击是通过恶意的Sql查询或者添加语句插入到应用的输入参数中，再在后台Sql服务器上解析执行进行的攻击。



## 二、Web程序三层架构

三层架构：界面层、业务逻辑层、数据访问层

区分层次目的：高内聚低耦合的思想

由数据库驱动的Web应用程序依从三层架构的思想也分为了三层： 

表示层：访问http://www.xxxx.com，呈现HTML内容

业务逻辑层（领域层）：加载、编译执行index.php，发送HTML文件

数据访问层（存储层）：执行SQL语句，返回数据

 三层架构是一种**线性关系**。 

 ![img](/images/assets/sql注入.jpg) 



## 三、Sql 注入漏洞详解

### Sql注入产生的原因及威胁

刚刚讲过当我们访问动态网页时, Web 服务器会向数据访问层发起 Sql 查询请求，如果权限验证通过就会执行 Sql 语句。
 这种网站内部直接发送的Sql请求一般不会有危险，但实际情况是很多时候需要**结合**用户的输入数据动态构造 Sql 语句，如果用户输入的数据被构造成恶意 Sql 代码，Web 应用又未对动态构造的 Sql 语句使用的参数进行审查，则会带来意想不到的危险。

Sql 注入带来的威胁主要有如下几点

- 猜解后台数据库，这是利用最多的方式，盗取网站的敏感信息。
- 绕过认证，列如绕过验证登录网站后台。
- 注入可以借助数据库的存储过程进行提权等操作

[查看具体实例]( https://www.jianshu.com/p/078df7a35671 )



## 四、判断Sql注入点

通常情况下，可能存在 Sql 注入漏洞的 Url 是类似这种形式 ：`http://xxx.xxx.xxx/abcd.php?id=XX`
 对 Sql 注入的判断，主要有两个方面：

- 判断该带参数的 Url 是否存在 Sql 注入？
- 如果存在 Sql 注入，那么属于哪种 Sql 注入？

可能存在 Sql 注入攻击的 ASP/PHP/JSP 动态网页中，一个动态网页中可能只有一个参数，有时可能有多个参数。有时是整型参数，有时是字符串型参数，不能一概而论。总之只要是带有参数的 动态网页且此网页访问了数据库，那么就有可能存在 Sql 注入。如果程序员没有足够的安全意识，没有进行必要的字符过滤，存在SQL注入的可能性就非常大。

### 4.1 判断是否存在Sql注入漏洞

经典：**单引号判断法**

在参数后面加上单引号，例如：

```cpp
http://xxx/abc.php?id=1'
```

 如果页面返回错误，则存在 Sql 注入。 

 原因是无论字符型还是整型都会因为单引号个数不匹配而报错。
（如果未报错，不代表不存在 Sql 注入，因为有可能页面对单引号做了过滤）



### 4.2判断Sql注入漏洞的类型

通常Sql注入漏洞的类型为2种类型：数字型和字符型

#### 4.2.1数字型的判断

当输入的参 x 为整型时，通常 abc.php 中 Sql 语句类型大致如下：
 `select * from <表名> where id = x`
 这种类型可以使用经典的 `and 1=1` 和 `and 1=2` 来判断：

- Url 地址中输入 `http://xxx/abc.php?id= x and 1=1` 页面依旧运行正常，继续进行下一步。
- Url 地址中继续输入 `http://xxx/abc.php?id= x and 1=2` 页面运行错误，则说明此 Sql 注入为数字型注入。



**分析**：

 当输入 `and 1=1`时，后台执行 Sql 语句： 

```reStructuredText
select * from <表名> where id = x and 1=1 
```

没有语法错误且逻辑判断为正确，所以返回正常。

当输入and 1=2时，后台执行Sql语句：

```reStructuredText
select * from <表名> where id = x and 1=2
```

 没有语法错误但是逻辑判断为假，所以返回错误。
我们再使用假设法：如果这是字符型注入的话，我们输入以上语句之后应该出现如下情况： 

```reStructuredText
select * from <表名> where id = 'x and 1=1' 
select * from <表名> where id = 'x and 1=2' 
```

 查询语句将 and 语句全部转换为了字符串，并没有进行 and 的逻辑判断，所以不会出现以上结果，故假设是不成立的。 





#### 4.2.2 字符型判断：

当输入的参 x 为字符型时，通常 abc.php 中 SQL 语句类型大致如下：
 `select * from <表名> where id = 'x'`
 这种类型我们同样可以使用and '1'='1'和 and '1'='1'来判断：

- Url 地址中输入 `http://xxx/abc.php?id= x' and '1'='1` 页面运行正常，继续进行下一步。
- Url 地址中继续输入 `http://xxx/abc.php?id= x' and '1'='2` 页面运行错误，则说明此 Sql 注入为字符型注入。

**分析：**

 当输入 `and '1'='1`时，后台执行 Sql 语句： 

```reStructuredText
select * from <表名> where id = 'x' and '1'='1'
```

语法正确，逻辑判断正确，所以返回正确。

当输入 `and '1'='2`时，后台执行 Sql 语句：

```reStructuredText
select * from <表名> where id = 'x' and '1'='2'
```

 语法正确，但逻辑判断错误，所以返回正确。 



[更多关于SQL的资料](https://sequel.jeremyevans.net/rdoc/files/doc/dataset_filtering_rdoc.html)