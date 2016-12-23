---
title: 前端自动化搭建流程
date: 2016-11-25
tags:
categories: nodejs
---

下面的步骤，都是在你电脑安装nodejs的前提下

### 安装yeoman[yeoman官网](www.yeoman.io)

```bash
npm install -g yo
```

<!-- more -->
### 在yeoman找到你需要的项目生成器,以angular为例

```bash
npm install -g generator-angular

```
### 查看你本地安装的generator

```bash
npm ls -g --depth=1 2>/dev/null | grep generator-

```
### 创建文件夹，然后初始化项目

```bash
yo angular
```
