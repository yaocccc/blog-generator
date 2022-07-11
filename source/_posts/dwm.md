---
title: dwm简介和如何安装使用
date: 2022-07-10 23:48:54
tags: [LINUX, DWM]
excerpt: 'dwm: what、why、how?'
index_img: /img/dwm/dwm.png
mermaid: false
---

## DWM

[DWM系列1——个人向DWM、arch linux展示](https://www.bilibili.com/video/BV1Ef4y1Z7kA/)

[DWM系列2——我为什么使用dwm、dwm初体验(最简化安装)](https://www.bilibili.com/video/BV1jN4y1u7ui/)

### WHAT

[dwm](https://dwm.suckless.org/)

dynamic window manager

是X上的一个动态窗口管理器

### WHY

轻、自定义、无限可能性

### HOW

1. 安装xserver环境(如果你未安装图形化环境的话)

    `sudo pacman -S xorg-xinit`

2. 克隆源码

    `git clone https://git.suckless.org/dwm`

3. make

    `sudo make clean install`

4. 修改 ~/.xinitrc

    `exec dwm`

5. 从tty进入 dwm

    `startx`
