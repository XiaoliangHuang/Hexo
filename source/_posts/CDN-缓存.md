---
title: CDN 缓存
date: 2017-04-05 10:53:22
tags: [CDN, 缓存]
categories: [技术]
---

```
-- CDN 是什么？
-- CDN 有什么作用？
-- CDN 怎么用？
-- CDN 的原理是什么？
-- 还有什么？
```


# CDN 概述 #

下面的这张Xmind图梳理了自己理解的关于CDN的知识图点，先从原理上介绍，然后通过一个实例了解到CDN的具体使用（试用具体产品的是了解一个东西很好的切入点），最后面对一些相关的扩展问题进行介绍。写几十博客的时候，使用Xmind之类的思维导图软件整理知识点是一种非常好的方法，通过这种层次性的关系，能够帮你理清楚思路。

![](cdn_knowledge_map.svg)

## 什么是CDN ##

CDN 的全称是 Content Delivery Network， 内容分发网络，是一种通过缓存服务器进行网站加速的方式。

CDN 解决的，是在复杂的网络情况下，很多地方用户访问网站速度慢的问题，其原理是在不同的地方建立缓存服务器，让用户就近访问。

<div align="center">
<img src="cdn.png" width="80%" align="center">
</div>

比如说，国内的宽带运营商有电信，网通，移动，长城等等，还有教育网，这些网络自己相互访问是很慢的，假如我是一个电信的用户，你的网站托管在了网通的机房里面，要直接访问网站，肯定慢得要死，但是如果把这个网站的内容在一个电信的服务器上面缓存起来，那么访问的时候，就不需要跨界到网通的线路上了，速度也自然会快起来。

## CDN的作用 ##

在知道了CDN 是什么以后，我们的下一个问题是：CDN有什么作用呢？

# Reference #

[CDN缓存那些事](https://www.qcloud.com/document/product/228/3236)
[OpenSSL SAN 证书](http://liaoph.com/openssl-san/)
[SSL 数字证书的标准、编码以及文件扩展名](https://kangzubin.com/certificate-format/)