---
title: Hexo 解决不能生成tags、categories的index页
date: 2018-02-07 16:39:37
categories:
- Hexo
tags:
- Hexo
---

### 问题
在使用next模板生成站点的时候，明明给文章打了标签，但是在站点启动，访问`http://xxxx:4000/tags/`或者`http://xxxx:4000/tags/categories/` 总是报错，找不到该链接；

![Cannot GET /categories](http://7xqzvt.com1.z0.glb.clouddn.com/16-4-28/17727126.jpg)


### 原因
模板next在生成tags和categories时，未在该文件夹下生成index.html；导致访问链接时报错；

### 解决方式

#### 方案一
> 在`source/`目录下分别创建`tags/`和`categories/`文件夹；并在文件夹中分别创建index.md文件；

```
$ mkdir -p blog/source/tags blog/source/categories
$ touch blog/source/tags/index.md
$ touch blog/source/categores/index.md
```


#### 方案二

通过终端执行

```
$ hexo new page categories
$ hexo new page tags
```

会在source文件夹创建两个文件夹categories, tags，并分别创建index.md文件

###### 目录结构
```
|- source
|-- tags
|	|- index.md
|--- categoroes
|	|- index.md
```

##### index.md 内容

###### tags/index.md内容
```
---
title: 标签
data: 2018-02-07 16:34
type: tags

---
```

###### categories/index.md内容
```
---
title: 分类
data: 2018-02-07 16:34
type: categories

---
```

完成上面操作；
重新生成&启动
```
$ hexo g
$ hexo s
```

http://localhost:4000/categories

![](http://7xqzvt.com1.z0.glb.clouddn.com/16-4-28/38418913.jpg)

