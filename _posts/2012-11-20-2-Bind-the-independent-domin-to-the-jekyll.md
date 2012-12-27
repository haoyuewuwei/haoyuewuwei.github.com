---
layout: post
title: "给jekyll绑定独立域名"
description: ""
category: Jekyll
tags: [经验, 独立域名]
---
{% include JB/setup %}

今天终于把自己的jekyll博客绑定了独立域名，一开始的时候，按照网上的教程始终是得不到需要的结果。最后发现其实是在tk的域名管理页面做的修改并没有即时生效，导致我以为我的操作没有效果，结果就在一次一次的修改。下面就总结下我个人在这次jekyll绑定独立域名中的收获吧：

两步走：

1. 在你的github仓库目录下新建一个CNAME文件，在里面写上你申请的免费域名，至于格式怎么写，其实你可以参照别人的，比如：[Other Jekyll websites](https://github.com/mojombo/jekyll/wiki/Sites).里面的source是很好的一个参照。

2. 在你新申请的域名管理空间的DNS解析里添加一条A Record，hostname就是你的免费域名，ip地址就是你的空间ip，可以采用ping username.github.com来活动哈。




补充知识（刚学的，如果有什么不对请指出^_^）：

A record：就是主机名和ip名的映射

CNAME：是两个主机名之间的映射（也成为一
个域名是另一个域名的别名）