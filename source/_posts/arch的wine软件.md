---
title: 适合arch linux的qq、微信、企业微信
date: 2022/07/05 00:46:20
tags: [LINUX, DWM, WINE]
excerpt: 适合arch linux的qq、微信、企业微信-wine
mermaid: false
index_img: /img/wine/wine.png
---

<!-- markdown-toc GitLab -->

* [适合arch linux的qq、微信、企业微信](#适合arch-linux的qq微信企业微信)
* [装完后可能会遇到的问题](#装完后可能会遇到的问题)
* [固定对应软件包的版本](#固定对应软件包的版本)
* [降级到指定的版本包](#降级到指定的版本包)

<!-- markdown-toc -->

## 适合arch linux的qq、微信、企业微信

```plaintext
  yay -S com.qq.tim.spark
  yay -S deepin-wine-wechat
  yay -S com.qq.weixin.work.deepin
```

## 装完后可能会遇到的问题

1. 微信聊天窗口中的字体变成方块  
    `cp WeiRuanYaHei-1.ttf ~/.deepinwine/xxx/drive_c/windows/Fonts`  
    这里的xxx是对应的软件目录  
    [文件下载地址](/file/menu)

2. 窗口周围出现奇怪的黑色或透明边框 只需要修改  
    ```plaintext
    /opt/apps/xxx/files/run.sh 中的 
    export APPRUN_CMD="deepin-wine6-stable"
    变更为
    export APPRUN_CMD="deepin-wine5"
    ```
    这里的xxx是对应的软件目录  

3. TIM启动后无法显示图片  
    sudo -S sysctl -w net.ipv6.conf.all.disable_ipv6=1

## 固定对应软件包的版本

```plaintext
  编辑 /etc/pacman.conf

  IgnorePkg = com.qq.weixin.work.deepin deepin-wine-wechat com.qq.tim.spark
```

## 降级到指定的版本包

找到包文件

用sudo pacman -U pkg.xx.xx 命令安装
