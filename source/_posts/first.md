---
title: 第一篇文章
date: 2018-03-21 13:33:30
tags:
---

博客从出现开始就一直流行着，不过它总是以各种各样的形式存在。现在很多服务商都提供些博客的平台，但是它们总是伴随着广告，让人很苦恼，所以越来越多的人尝试着通过开源项目和平台搭建专属博客。

<!--------more----------->

我也尝试了很多种方式，比如在heroku免费空间上托管wordpress，在空闲主机上搭建wordpress，用github+hexo来托管静态网站等。不过它们都有很多不便。

* wordpress功能比较好，但是体积比较大，在heroku上托管时如果30分钟不访问就会关闭项目。当然能通过网站监控等解决这个问题，但依然麻烦。
* 自己在空闲主机上搭建，虽然能保证自己数据比较安全，但是由于80端口443等端口被封还有网速等原因也是麻烦，如果购买vps也不划算。
* 在GitHub上托管hexo，无法自动部署，还无法异地，需要借助第三方处理。

综上分析，现在选择gitlab+hexo进行托管。gitlab自带ci部署，这点真的方便了很多。

至于如何搭建，以后再说。这篇只是在高铁上发闷写着玩的。