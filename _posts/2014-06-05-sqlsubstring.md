---
date: 2014-06-05 11:27:00
title: mysql下数据的截取
layout: post
tags:
   - mysql
   - substring
categories:
   - Super xing
---

##  在mysql数据库种截取自负串

＋ 今天在进行数据迁移的时候要对一些数据进行修改，因为文件的地址不一样，所以要修改，

＋ <b>1232432423423.pdf</b> 修改未  <b>1232432423423</b>

+ 如何才能才能在数据库种将后缀名去掉呢？


### ps:  update table set `key`=SUBSTRING(`key`,4,LENGTH(`key`)-4)

  + 截取字符串的后四位，success
