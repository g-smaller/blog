---
title: Hexo 搭建
date: 2017-08-11 22:10:00
categories:
- Hexo
tags: 
- Hexo
- Node.js
---

### 前言

一直想搭建一个个人技术微博，之前接触过Hexo，但是没有深入了解。

今天闲暇之余，看了关于Hexo的文章，发现在搭建个人技术博客方面是一个很成熟的平台工具。快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。于是试着学习并搭建了一个个人技术博客，以下环境搭建；

[更详细的博客搭建教程](http://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html)

[Hexo官方文档](https://hexo.io/zh-cn/docs/index.html)

官方文档可以让你事半功倍

<!-- more -->

### 安装
- nvm
- Node.js
- [Hexo](https://hexo.io)

#### 安装`nvm`

使用管理工具 [`nvm`](https://github.com/creationix/nvm)

有关Node管理工具的介绍：http://weizhifeng.net/node-version-management-via-n-and-nvm.html

通过脚步安装

```bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
or

```bash
$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

#### 安装`Node.js`

```bash
nvm install stable
```

安装成功运行 `node` 如果出现控制台，说明安装成功；如果出现`zsh: command not found: node`，运行`nvm use 8.3.0` 其中8.3.0是我本地安装Node的版本；

以下的提示是我使用`brew`安装`nvm`，使用`nvm`安装Node报的错误。
[issue](https://github.com/creationix/nvm/issues/855)
如果你遇到了以下错误，请使用脚步安装`nvm`
```
nvm is not compatible with the npm config "prefix" option: currently set to "/usr/local/Cellar/nvm/0.33.2/versions/node/v8.3.0"
Run `nvm use --delete-prefix v8.3.0` to unset it.
```
[正确的安装和使用nvm(Mac)](http://www.imooc.com/article/14617)

验证
```bash
$ nvm current
v8.3.0
```

#### 安装`Hexo`

``` bash
$ npm install -g hexo-cli
```

安装过程成由于网络原因或者资源请求不到，导致安装Hexo失败。

解决的办法就是设置代理: 
```bash
$ npm config set registry https://registry.npm.taobao.org
```

### 搭建博客

搭建了半天的环境，接下来就试试Hexo搭建博客的神奇之处。

#### 建站

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

更多说明：[Setup](https://hexo.io/zh-cn/docs/setup.html)

#### 配置

更多说明：[Configuration](https://hexo.io/zh-cn/docs/configuration.html)

#### 创建一个新博客

``` bash
$ hexo new "博客名称-文件名"
```

更多说明：[Writing](https://hexo.io/zh-cn/docs/writing.html)

### 启动服务

``` bash
$ hexo server
```

更多说明：[Server](https://hexo.io/zh-cn/docs/server.html)

### 渲染静态文件

``` bash
$ hexo generate
```

更多说明：[Generating](https://hexo.io/zh-cn/docs/generating.html)

### 部署到远程站点

``` bash
$ hexo deploy
```

更多说明：[Deployment](https://hexo.io/zh-cn/docs/deployment.html)
