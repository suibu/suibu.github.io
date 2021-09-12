layout: post
read_time: true
show_date: true
title: '组件库打包'
date: 2021-07-19
img: posts/20210705/4.jpg
tags: [工程化]
category: opinion
author: 随步
description: '组件库库打包'
---
Mysql Sequelize
mysql是关系型数据库

Mongodb Mongoose

Redis
多核CPU擅长处理多进程任务，所有web server也都是多进程。无论PM2，nginx还是其他。但进程之间有内存隔离，所以需要第三方缓存服务。
类似于用户A第一次访问进程1，东西如果暂存在内存片段1中，第二次访问进入了进程2，由于内存隔离，进程2又需要在缓存一次。所以需要redis这样的第三方缓存服务去存储。
![image.png](https://upload-images.jianshu.io/upload_images/6195923-2d2f0e0442c48f68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
