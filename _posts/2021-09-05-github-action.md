layout: post
read_time: true
show_date: true
title: 'github action'
date: 2021-07-19
img: posts/20210705/4.jpg
tags: [cli]
category: opinion
author: 随步
description: 'github action'
---

```
name: //语义化的名字
on: // 触发条件
  push:
    branches: [ master ] // 分支
    paths: ['src/**'] // 这些文件修改才触发
        
  pull_request:
    branches: [ master ]

jobs:
    test:
        runs-on: ubuntu-latest // 在ubuntu 操作系统上跑
        steps: // 写几个-，就有几个步骤
            - uses: actions/checkout@v2 // 用第三方的actions，git pull 的意思
            - name: Use Nodejs
              uses: actions/setup-node@v1 // 安装nodejs
              with: node-version: 14
            - name: print node version
              run: |
                node -v && npm -v && echo 'hi github actions'
    test2:
        runs-on: ubuntu-latest
        steps:
            - run: touch a.txt
            - run: echo '输入文本到 a.txt' > a.txt
            - run: cat a.txt

 
```