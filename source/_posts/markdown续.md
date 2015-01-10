title: Markdown续
date: 2014-12-30 19:56:50
tags: [Markdown,Hexo]
categories: Hexo	
---
 这两天学了hexo以及markdown，然后自己弄了个个人博客，可是发现里面东西好单调，偶然想起来貌似有些网站里面可以有音乐视屏什么的，于是网上了解了一下在博文中加入图片、音频以及视屏。

## *图片*
代码如下：

`	![](http://ww2.sinaimg.cn/large/5e8cb366jw1e62o63tkv3j20dh078q5a.jpg)`

下面是效果图：

![](http://ww2.sinaimg.cn/large/5e8cb366jw1e62o63tkv3j20dh078q5a.jpg)


## *音乐*

以『虾米音乐』为例，歌曲页面有个『转帖』选项，将html代码或javascript代码复制到文中即可。

代码如下：

    <script type="text/javascript" src="http://www.xiami.com/widget/player-multi?uid=45521081&sid=1770417636,&width=235&height=346&mainColor=FF8719&backColor=494949&autoplay=0&mode=js"></script>

效果图如下：

<script type="text/javascript" src="http://www.xiami.com/widget/player-multi?uid=45521081&sid=1770417636,&width=235&height=346&mainColor=FF8719&backColor=494949&autoplay=0&mode=js"></script>


## *视屏*

嵌入视频的方法和音乐类似，视频网站每个视频页面都会有一个『分享』或『转帖』按钮，点击可以查看代码。

代码如下：

    <iframe height=498 width=510 src="http://player.youku.com/embed/XMjI2MjU3MDMy" frameborder=0 allowfullscreen></iframe>

效果：


<iframe height=498 width=510 src="http://player.youku.com/embed/XMjI2MjU3MDMy" frameborder=0 allowfullscreen></iframe>



原文链接：

[怎样在博文中嵌入图片、音乐、视频？](http://zipperary.com/2013/06/27/media-on-hexo/)
