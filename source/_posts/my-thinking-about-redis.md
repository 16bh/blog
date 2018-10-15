---
title: 关于缓存的一些思考
categories:
tags: [redis]
toc: false
comment: true
date: 2017-04-12 14:12:41
---

<img src="/images/20170728150122388188040.png" width="492" height="297"/>

> 性能有问题怎么办，加机器，做缓存


<!--more-->

## redis和memcached
区别：memcached只支持简单的key-value格式，redis支持更多的格式，如hash,


# 什么时候要用到缓存

列表页、详情页等经常请求而且变化频率不高的场景


# 缓存基本用法

1. 从缓存中取数据
2. 缓存命中，直接返回
3. 缓存miss，从db中取数据，将数据存入缓存，返回



# 缓存穿透

指的是访问缓存中的某个不存在的key，没有命中，然后直接请求db，同时因为返回的结果为空，不会存入缓存中，故而下次还是无法命中的场景

解决方案：将不存在的key也存入缓存中，value设置为null

# 持久化

# 缓存并发




# 缓存过期时间

存入缓存时可以设置缓存的过期时间

有一个要注意的地方在于如果有一批过期时间相同的key同时过期并请求db时，会使db的压力骤增，如果扛不住，就会导致缓存雪崩

处理方法：设置过期时间时加上一个随机的小数字

# 缓存是否过期

是否给缓存设置过期时间，也是个值得思考的问题？

一般有两种思路：
一种是设置过期时间，到期后重新生成新的db
另一种是不设置过期时间，手动更新缓存

一般是采用第一种，因为redis存储的空间是有限的

此外，一些需要持久的缓存是不会设置过期时间的，如最新的订单号等

# redis缓存的其他应用场景

## 队列
使用`redis`可以构建简单的一对一的队列，简单的说，就是使用`lpush`入列，`rpop`出列（也可以右进左出）

## 数据库
是的，你没有看错，`redis`除了当缓存使用，作为`NoSql`的成员之一，它还可以当独立的数据库使用


# 参考

>[缓存穿透、并发和失效，来自一线架构师的解决方案](http://mp.weixin.qq.com/s?__biz=MzA5Nzc4OTA1Mw==&mid=2659597537&idx=1&sn=9c91d231315b507b5eaea0e465a01423&scene=21#wechat_redirect)
>
>[如何解决常见的缓存穿透、并发和失效问题？](http://mp.weixin.qq.com/s/CCRa-qbgnNYSI4b10q4F9g)