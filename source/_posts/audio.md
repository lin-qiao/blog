---
title: audio对象身上的属性方法和事件
date: 2016-08-05 17:04:35
tags:
categories: html5
---
一个audio对象就是普通的dom对象
比其他的dom对象多出一些自己独有的属性和方法

<!-- more -->

##属性
* audio.volume 读写音量
* audio.src    读写地址
* audio.currentTime 读写歌曲当前的播放时常
* audio.duration 读 歌曲的总长度
* audio.paused 只读 布尔类型 是否处于暂停状态
* audio.ended  只读 布尔类型 是否播放完毕
##方法
audio.play()   让歌曲开始播放
audio.pause()  歌曲暂停
##事件
audio.oncanplay=fn()  当歌曲下载完之后调用fn
audio.onvolumechange=fn() 当audio.volume变换时调用
audio.onplay=fn()          当歌曲开始后调用
audio.onpause =fn() 歌曲暂停时调用
audio.ontimeupdate=fn() 在播放过程中一直调用
audio.onended = fn() 一首歌播放完之后调用
