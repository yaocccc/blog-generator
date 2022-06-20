---
title: linux丝滑的动画体验
date: 2022/06/19 20:42:48
tags: [LINUX, PICOM, 桌面美化]
excerpt: xserver的窗口效果器 动画、透明、模糊、圆角
mermaid: false
---

<!-- markdown-toc GitLab -->

* [picom](#picom)
* [安装](#安装)
* [启动](#启动)
* [展示](#展示)

<!-- markdown-toc -->

## picom

xserver的窗口效果合成器, fork自 compton

提供了窗口的透明、背景模糊、淡入淡出、圆角、动画等效果

[github仓库](https://github.com/yshui/picom) | [个人fork版本](https://github.com/yaocccc/picom)

[个人自用配置](https://github.com/yaocccc/scripts/blob/master/config/picom.conf)

个人fork的版本修改了什么？

在dccsillag/picom实现动画效果的基础上，增加了两个动画相关的配置 [PR#35](https://github.com/dccsillag/picom/pull/35)

## 安装

从仓库clone到本地

```shell
cd picom
git checkout implement-window-animations

rm -rf build
LDFLAGS="-L/usr/local/lib" CPPFLAGS="-I/usr/local/include" meson --buildtype=release . build
ninja -C build
sudo ninja -C build install
```

## 启动

`picom --experimental-backends --config ~/scripts/config/picom.conf`

## 展示

[视频链接](https://www.bilibili.com/video/bv19T411G7Eq)
