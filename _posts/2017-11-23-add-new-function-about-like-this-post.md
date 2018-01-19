---
layout: post
title: 为博客新增了文章点赞的功能
date: 2017-11-23
categories:
- 日常
tags: [jekyll博客]
status: publish
type: post
published: true
author:
  login: PriestTomb
  email: mxingzh@163.com
  display_name: PriestTomb
---

## 写在前面

目前虽然给博客加了评论功能和访客记录(在 lean cloud 云数据库中存着)，但发现没人评论，就一时兴起想再加个 👍 和 👎 的功能，为那些懒得评论的人服务

## 初期设计

先简单的想了下，认为就和访客记录类似，把 like (👍) 和 dislike (👎) 存到数据库里，点过的人，加载页面的时候，图标展示不一样，再点一次可以取消，blablabla

## 两个问题

到开发的时候渐渐有两个问题：

* 两个按钮的逻辑到底怎么规定？如果先点了 👍 ，那还能不能点 👎 ？如果能点，那是不是就要自动把 👍 的记录取消？如果不能点，那是不是得麻烦先再点一次 👍，重置了状态之后才能点 👎？

* 数据库里怎么设计？要新建一个 访客点赞 表么？关联访客表和文章表(其实我也没有文章表)

## 重新捋逻辑

嫌 👎 功能加进来太复杂，而且看了下目前几个博客网站，其实都只有点赞按钮，索性就只做一个点赞好了

结合之前的 IP 判断、访客记录增加等功能，重新捋了下逻辑：

![整体流程](http://oxujjb0ls.bkt.clouddn.com/image/%E5%8D%9A%E5%AE%A2%E6%96%B0%E5%A2%9E%E7%82%B9%E8%B5%9E%E5%8A%9F%E8%83%BD/%E6%96%B0%E7%9A%84%E6%B5%81%E7%A8%8B.png)

## 改造数据库表

之前两张表：

* visited_times : 访问次数表

* visitors_record : 访客记录表

现在要把点赞加上去，第一反应是在访客记录表中补一个字段，因为这张表本身就有了 IP、访问文章 两个字段，用这两个字段简单确定一个访客有没有对某篇文章点过赞最简单不过了（虽然这样的设计太随便了）

但还要有个每篇文章的点赞数，这个难道要加在访问次数表里？好别扭....想了一下，干脆把访问次数表改造成 Posts 表，即文章表，文章表有两个字段：访问次数、点赞数，这样就刚好了（虽然这样设计也是有点随便）

## 代码

重新写了几个方法，把原来增加访问次数的方法名称、变量命名之类的都改掉了，详细的可以看我这次的[提交记录](https://github.com/PriestTomb/PriestTomb.github.io/commit/dca29503381454eab800754857fbb2194f1287b8)

## 实现效果

![实现效果](http://oxujjb0ls.bkt.clouddn.com/image/%E5%8D%9A%E5%AE%A2%E6%96%B0%E5%A2%9E%E7%82%B9%E8%B5%9E%E5%8A%9F%E8%83%BD/%E5%AE%9E%E7%8E%B0%E6%95%88%E6%9E%9C.png)

## 最后

😭 观察一段时间看看有没有收获点赞，有没有发现新BUG哈哈