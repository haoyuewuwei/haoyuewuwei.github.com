---
layout: post
title: "Jekyll中支持代码高亮以及插入图片"
description: "Jekyll中支持代码高亮以及插入图片"
category: Jekyll
tags: [代码高亮, jekyll, 个人博客]
---
{% include JB/setup %}

既然使用了Jekyll来作为自己的个人博客，那么在博客里面贴代码肯定是不可避免的，具体方法总结如下：
1. jekyll个人博客代码高亮的方法有多种，具体可以参考：[Jekyll的代码高亮](http://note.sdo.com/u/1540150068/n/rPdcQ~jHv3h9nM0jI000iy)
2. 个人采用的方法是上述所说的第二点，利用Pygments功能：关于该方法的详细介绍详见[这里](http://stackoverflow.com/questions/13464590/github-flavored-markdown-and-pygments-highlighting-in-jekyll)

然后就可以在博客里插入代码啦，如下
{% highlight c %}
int main()
{
	puts("hello,world!");
}
{% endhighlight %}

另外就是关于在博客中添加图片的方式，因为markdown本来就支持内嵌html语言，所以直接利用html格式就好了，这里给大家推荐一下微软的skydrive，这个软件可以自动生成图片内嵌的html代码，下图：
<iframe src="https://skydrive.live.com/embed?cid=90ABA068241662DC&resid=90ABA068241662DC%21109&authkey=AJFm22QygqmI5ls" width="156" height="319" frameborder="0" scrolling="no"></iframe>
非常方便快捷哈，另外还有一种方式是直接把图片传到github上面，然后修改config.yml里面的图片路径，个人还是偏向于用skydrive哈！