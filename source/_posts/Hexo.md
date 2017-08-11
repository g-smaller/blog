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

以下是使用Mac brew安装的

安装[`brew`](https://brew.sh)

```bash
$ brew install nvm
$ ln -s /usr/local/Cellar/nvm/$version ~/.nvm
```

> $version 是nvm安装版本

##### 设置环境

>添加配置到`~/bash_profile` `~/.zshrc` `~/.profile` `~/j.bashrc` 四个文件其中任意一个

> 我使用的shell是[`zsh`](http://ohmyz.sh)

```bash
$ vim .zshrc

## 把以下脚本添加到文件末尾
export NVM_DIR="~/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

> 编辑完成不要忘记 `source .zshrc`

#### 安装`Node.js`

```bash
nvm install stable
```

安装成功运行 `node` 如果出现控制台，说明安装成功；如果出现`zsh: command not found: node`，运行`nvm use 8.3.0` 其中8.3.0是我本地安装Node的版本；

```
nvm is not compatible with the npm config "prefix" option: currently set to "/usr/local/Cellar/nvm/0.33.2/versions/node/v8.3.0"
Run `nvm use --delete-prefix v8.3.0` to unset it.
```
如果出现这种问题， 按提示运行 `nvm use --delete-prefix v8.3.0`

1. 出现提示信息：`Now using node v8.3.0 (npm v5.3.0)` 

2. 运行 `nvm current` ， 出现 `v8.3.0` 

> 运行脚本出现提示信息，Node环境配置成功


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
