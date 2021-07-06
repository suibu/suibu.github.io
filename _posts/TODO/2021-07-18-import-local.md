---
layout: post
read_time: true
show_date: true
title: 'yargs工作流程'
date: 2021-07-05
img: posts/20210705/4.jpg
tags: [node,cli]
category: opinion
author: 随步
description: 'import-local'
---


#### import-local的作用

```javascript
const importLocal = require('import-local');
/*
 __filename 等价于 which 包名，例如 which lerna， 返回的地址，会有软链接映射到本地的文件
 如果importLocal(__filename)返回true，会优先加载本地版本
*/
if (importLocal(__filename)) {
	console.log('Using local version of this package');
} else {
	// Code for both global and local version here…
    require(".")(process.argv.slice(2));
    // require(".")代表当前文件夹的index.js
}
```
运行node项目时候，默认会向上下文中注入变量：module、require、__filename、__dirname、exports
是在 require的时候注入的。

```javascript
'use strict';
const path = require('path');
const resolveCwd = require('resolve-cwd');
const pkgDir = require('pkg-dir');
// pkg-dir帮助我们，给定一个目录，帮我们获取包含package.json的上级目录

module.exports = filename => {
	// path.dirname(filename) 会一层层向上找包含package.json的模块目录，例如传入 /node/v14.16.0/lib/node_modules/lerna/cli.js，逐层找到就近的package.json 所在的文件夹
    // sync可以在module.export 中多返回一个方法
	const globalDir = pkgDir.sync(path.dirname(filename));
	// 将globalDir, filename做一个相对路径，相当于filename的路径减去globalDir所得的中间路径
	const relativePath = path.relative(globalDir, filename);
	// 将 globalDir与package.json进行合并，取到package.json的文件内容
	const pkg = require(path.join(globalDir, 'package.json'));
	// localFile取到源码的路径
	const localFile = resolveCwd.silent(path.join(pkg.name, relativePath));
	const localNodeModules = path.join(process.cwd(), 'node_modules');
	const filenameInLocalNodeModules = !path.relative(localNodeModules, filename).startsWith('..');

	// Use `path.relative()` to detect local package installation,
	// because __filename's case is inconsistent on Windows
	// Can use `===` when targeting Node.js 8
	// See https://github.com/nodejs/node/issues/6624
	return !filenameInLocalNodeModules && localFile && path.relative(localFile, filename) !== '' && require(localFile);
};

```
// TODO: 补充
##### path.resolve()和path.join()的区别
path.resolve()相当于cd的功能。
path.resolve('/Users', '/suibu', '..')，等价与cd /Users, cd /suibu, cd ..
path.join()相当于拼接的功能。
path.join('/Users', '/suibu', '..')，等价与cd /Users/suibu , cd ..

##### path.parse(dir) 
帮我们解析当前路径

// TODO: 补充
#### node.js 模块路径解析流程
