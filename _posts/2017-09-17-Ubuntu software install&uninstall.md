---
layout: post
title: 'Ubuntu下软件的安装与卸载'
subtitle: 'Linux'
date: 2017-9-17
categories: Linux
tags: Ubuntu Linux
---
# Ubuntu下软件的安装与卸载
初探`linux`真是让人抓狂，软件的安装也令人头疼。终端下软件的成功安装也没有一个明确的提示，让我一直以为是无缘无故安装的莫名终止。。。说多了都是泪(╯﹏╰)。  
## 软件安装
暂时只更新`deb`软件的安装，其他类型的遇到再更新。  
使用命令：
> sudo dpkg -i 软件名称.deb

来安装软件。如果提示某些依赖不存在，可以使用命令：
> sudo apt-get install -f

来自动的查找安装相关依赖。（简直超方便）

## 软件卸载
1. 如果你知道要删除软件的具体名称，可以使用  
> sudo apt-get remove --purge 软件名称  
sudo apt-get autoremove --purge 软件名称 

2. 如果不知道要删除软件的具体名称，可以使用
> dpkg --get-selections \| grep ‘软件名称关键字’

来查找软件的具体名称，然后使用第一个方法进行卸载。
