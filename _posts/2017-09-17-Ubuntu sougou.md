---
layout: post
title: 'Ubuntu下搜狗输入法无法实现中英文切换'
subtitle: 'Linux'
date: 2017-9-17
categories: Linux
tags: Ubuntu Linux 输入法
---
# Ubuntu下搜狗输入法无法实现中英文切换
莫名其妙的按 __shift__ 键无法实现中文向英文的转换，即使进行了下图的设置:  
<img src="/assets/img/sougou.png">
## 解决方法
问题是在fcitx下输入法配置的问题，使用命令：  
> fcitx-config-gtk3    

查看输入法配置，正常情况下应该有中文和英文两种输入法。所以只有中文是没办法进行切换的。 
点击最下角'+'号进行添加即可。
<img src="/assets/img/sougou1.png">  

然后按 __shift__ 键就有效了～～～

