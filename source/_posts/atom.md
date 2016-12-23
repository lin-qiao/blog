---
title: Atom编辑器安装及使用基础
date: 2016-12-23
tags:
categories: 编辑器
---

## 为什么选择使用Atom

Atom是GitHub推出的一款编辑器, 被称为21世纪的黑客编辑器.作为一个现代的代码编辑器，Atom 有着各种流行编辑器都有的特性，功能上非常丰富，支持各种编程语言的代码高亮(HTML / CSS / Javascript / PHP / Python / C / C++ / Objective C / Java / JSON / Perl / CoffeeScript / Go / Sass / YAML / Markdown 等等)、 与大多数其他编辑器相比，Atom的语言支持已经算是覆盖非常全面了。另外，它的代码补全功能（也叫Snippets） 也非常好用，你只需输入几个字符即可展开成各种常用代码，可以极大提高编程效率。

<!-- more -->
## 安装

[打开官方主页](https://atom.io/)

网页会自动判断你的操作系统, 给出其对应的下载按钮
![Image text](https://raw.githubusercontent.com/PeterHo/images/master/blog/editor/atom/atom_1/linux-downloads.png)
## 基本使用

### 命令面板

Atom的很多功能学习和参考了其他优秀的编辑器, 命令面板就是其一.
当你第一次看到它时, 还以为在用Sublime呢
命令面板是Atom中最常用的功能之一, 当你在编辑器中使用快捷键Ctrl+Shift+P时, 就会看到它
![Image text](https://raw.githubusercontent.com/PeterHo/images/master/blog/editor/atom/atom_1/command-palette.png)
在控制面板中可以输入Atom中和插件中定义的所有命令, 并且支持模糊搜索
比如说当你输入cboo时, 所有包含有这4个字符的命令就都列出来了
在列出的命令后还显示了此命令对应的快捷键(如果有的话)

## 设置窗口

自带可视化的设置界面是Atom使用很方便的原因之一, 而不像传统的编辑器那样需要手动修改配置文件.

![Image text](https://raw.githubusercontent.com/PeterHo/images/master/blog/editor/atom/atom_1/settings.png)

你可以使用下面三种方法来打开设置窗口

* 主菜单Edit->Preferences
* 在命令面板中输入命令Settings View:Open. 因为命令窗口支持模糊查询, 因此只需要输入svo, 就可以了
* 使用快捷键Ctrl+,

在设置窗口中可以设置和管理各种编辑器行为, 键盘快捷键, 插件, 主题等内容

## 设置窗口界面主题和代码高亮

Atom自带了4种窗口主题和8种代码高亮方式

可以通过设置窗口中的Themes页面来配置和修改

![Image text](https://raw.githubusercontent.com/PeterHo/images/master/blog/editor/atom/atom_1/theme.png)

## 安装插件

通过设置窗口Settings界面的Install选项中，可以搜索插件或者主题进行下载安装

![Image text](http://img.blog.csdn.net/20151212191447866)

## 常用的一些插件

### 1、simplified-chinese-menu
Atom的简体中文语言包，完整汉化，兼容所有已发布的版本Atom。

### 2、linter-js-standard
用来使javascript代码格式化。
### 3、emmet
这款插件是用来支持zend-coding，Emmet的前身是大名鼎鼎的Zen coding，如果你从事Web前端开发的话，对该插件一定不会陌生。它使用仿CSS选择器的语法来生成代码，大大提高了HTML/CSS代码编写的速度
### 4、autoclose-html
html标签自动比较。
### 5、atom-ternjs
该插件能对一个对象中拥有的对外提供的属性和方法都能通过suggest的形式提示出来，能对一个对象对外提供的接口有一个选择过程，可以理解为js代码自动提示。
### 6、atom-bootstrap3
bootstrap3代码提示插件。
### 7、autocomplete-paths
文件路径自动提示。

![Image text](http://upload-images.jianshu.io/upload_images/1980884-9aa1f194f81ff3ef.gif?imageMogr2/auto-orient/strip)

### 8、jquery-snippets
jquery代码提示，安装完之后要重新启动Atom。

### 9、autoprefixer
css代码前缀自动补全。

![Image text](http://upload-images.jianshu.io/upload_images/1980884-d4c97a1b1e7c2a86.gif?imageMogr2/auto-orient/strip)
### 10、color-picker
取色器。

![Image text](http://upload-images.jianshu.io/upload_images/1980884-deb030a911e29e87.gif?imageMogr2/auto-orient/strip)

### 11、activate-power-mode
编写代码特效

![Image text](http://upload-images.jianshu.io/upload_images/1980884-f92385d3975ba2a1.png?imageMogr2/auto-orient/strip)

### 12、atom-ternjs
JS代码智能提示。

### 13、snippets
按tab键自动填充代码。
