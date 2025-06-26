---
title: Nodejs Start
tags:
  - study
  - Nodejs
categories:
  - Nodejs学习
date: 2025-03-04 17:28:37
---
从今天开始我要开始学习Nodejs，虽然这东东好像也不用特意去学习的，但是我感觉还是有必要过一遍，就当学一点前端技术了。
<!--more-->
## 编写”Hello world“

```node
var http = require('http');

  

http.createServer(function (request, response) {

  

        // 发送 HTTP 头部

        // HTTP 状态值: 200 : OK

        // 内容类型: text/plain

        response.writeHead(200, {'Content-Type': 'text/plain'});

  

        // 发送响应数据 "Hello World"

        response.end('Hello World\n');

}).listen(7888);

  

// 终端打印如下信息

console.log('Server running at http://127.0.0.1:7888/');
```
以上是代码。
![报错](../attachment/Pasted%20image%2020250304205234.png)
然后我就报了一个这样的错。直接Google报错信息，就可以知道这是因为端口被占用了。当我在搜索解决方法的时候，找到了这样一个指令：
```bash
 netstat -aon | FINDSTR "8888"
```
这似乎可以查找占用该端口的进程，但是非常奇怪，我执行这个命令之后没有任何的输出。后来又搜索了一下`netstat`指令，发现好像在Windows系统上没有这个`-aon`的指令，只有`-r -n -s`，具体也可以`netstat /?`查看。
![](../attachment/Pasted%20image%2020250304210514.png)
所以我觉得还是蛮神奇的，这可能是Linux指令，虽然我在Ubuntu上运行这个指令还报错了FINDSTR这个指令不存在。总之非常奇怪的。最后我也没搞懂，铺天盖地都是这个指令，但是我就是运行不了。
最后靠着菜鸟教程解决了：
![](../attachment/Pasted%20image%2020250304210707.png)
把代码里的”8888”改成“7888”就可以了。
![](../attachment/Pasted%20image%2020250304210749.png)
然后访问**127.0.0.1:7888**就能看到`Hello World`了。

## NPM的使用

其实对我来说，npm已经算是老老朋友了。自从有了博客之后，每天都要跟这个玩意儿斗智斗勇。今天还刚被package.json折磨，不过不是什么大事，只不过是在重新整理博客的主文件夹。
### npm全局安装和本地安装

这里用到的实例是：
```bash
npm install express
```
不过这样安装好之后，是将express包放在工程目录下的`node_modules`目录中的。
而全局安装会安装在系统级别的目录中。
以下是我示范的全局安装，安装完成使用`npm list -g`查看。因为我使用了fnm管理npm的版本，所以显示的路径是在fnm的相关路径上的。
![](../attachment/Pasted%20image%2020250304212214.png)
可以看到成功安装。当然也可以使用`npm list express -g`查看express包的具体版本号，一定要加上`-g`，因为我本地没有express包。

### 常用指令
1. 更新模块：
```bash
npm update express
```
2. 搜索模块
```bash
npm search express
```
3. 生成`package.json`文件
```bash
npm init
```
4. 在 npm 资源库中注册用户
```bash
npm adduser
```
5. 发布模块
```bash
npm publish
```
当然这些常用指令都是可以`npm help`查看的。

## package.json的说明与使用

### package.json文件简介

```json
{

  "name": "hexo-site",

  "version": "0.0.0",

  "private": true,

  "scripts": {

    "build": "hexo generate",

    "clean": "hexo clean",

    "deploy": "hexo deploy",

    "server": "hexo server"

  },

  "hexo": {

    "version": "7.3.0"

  },

  "dependencies": {

    "hexo": "^7.3.0",

    "hexo-algoliasearch": "^2.0.1",

    "hexo-deployer-git": "^4.0.0",

    "hexo-generator-archive": "^2.0.0",

    "hexo-generator-category": "^2.0.0",

    "hexo-generator-index": "^4.0.0",

    "hexo-generator-search": "^2.4.3",

    "hexo-generator-tag": "^2.0.0",

    "hexo-renderer-ejs": "^2.0.0",

    "hexo-renderer-marked": "^6.3.0",

    "hexo-renderer-pug": "^3.0.0",

    "hexo-renderer-stylus": "^3.0.1",

    "hexo-server": "^3.0.0",

    "hexo-theme-butterfly": "^5.2.2",

    "hexo-theme-landscape": "^1.0.0",

    "hexo-theme-next": "latest",

    "hexo-util": "^3.3.0",

    "hexo-wordcount": "^6.0.1",

    "js-yaml": "^4.1.0",

    "moment-timezone": "^0.5.46",

    "node-sass": "^9.0.0",

    "typings": "^2.1.1"

  }

}
```
以上是我的hexo博客的package.json文件，可以很容易看出来各个字段的含义。

- `name`：项目的名称，应该是唯一的，通常使用小写字母和连字符。
- `version`：项目的版本号，遵循语义化版本控制（Semantic Versioning）。
- `description`：项目的简短描述。
- `main`：项目的入口文件，通常是应用程序的启动文件。
- **`scripts`**：定义了一系列的命令行脚本，可以在项目中执行特定的任务。
- **`dependencies`**：列出了项目运行所需的所有依赖包及其版本。
- `devDependencies`：列出了只在开发过程中需要的依赖包及其版本。
- `peerDependencies`：列出了项目期望其依赖包也依赖的包。
- `optionalDependencies`：列出了可选的依赖包。
- `engines`：指定了项目兼容的 Node.js 版本。
- `repository`：项目的代码仓库信息，如 GitHub 仓库的 URL。
- `keywords`：项目的关键词，有助于在 npm 搜索中找到项目。
- `author`：项目的作者信息。
- `license`：项目的许可证信息。

当然有些字段我上面的文件中也没有。
我们在使用 `npm install <package-name>` 命令安装依赖后，npm 会自动将依赖添加到 `package.json` 文件的 `dependencies` 或 `devDependencies` 中，并创建 `package-lock.json` 文件以锁定依赖的版本。因此，`package.json`文件就是一个管理项目的环境依赖的文件。

### 使用 package.json 的好处

- **依赖管理**：集中管理项目的依赖项及其版本。
- **自动化任务**：通过 `scripts` 字段可以方便地运行常见的任务。
- **版本控制**：确保项目及其依赖版本的一致性，便于团队协作。
- **描述项目**：为项目提供元数据信息，便于发布和共享。

## ENDING

前置知识就是这些了，之后就开始做简单应用了。