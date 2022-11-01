---
title: rofi实现自定义菜单选项和操作、像fzf一样使用rofi
date: 2022/06/19 20:42:17
tags: [LINUX, DWM, ROFI, 桌面美化]
excerpt: rofi实现自定义菜单选项和操作、像fzf一样使用rofi
mermaid: false
index_img: /img/rofi/rofi.png
---

<!-- markdown-toc GitLab -->

* [ROFI](#rofi)
* [用自定义脚本实现 rofi自定义菜单和选项](#用自定义脚本实现-rofi自定义菜单和选项)
* [rofi dmenu模式的使用](#rofi-dmenu模式的使用)
* [自用的rofi主题](#自用的rofi主题)

<!-- markdown-toc -->

## ROFI

可观看bilibili视频 [BV1ag411Q7Po](https://www.bilibili.com/video/BV1ag411Q7Po/)

rofi--一个窗口切换器、应用启动器、dmenu的替代品

[git仓库](https://github.com/davatorium/rofi#modes)

## 用自定义脚本实现 rofi自定义菜单和选项

自己自定义菜单项 和 选中后的操作

[自定义脚本](https://github.com/davatorium/rofi/blob/next/doc/rofi-script.5.markdown)

```plaintext
  rofi -show 自定义 -modi "自定义:~/rofi.sh"
    1: 上述命令可调用rofi.sh作为自定义脚本
    2: 将打印的内容作为rofi的选项
    3: 每次选中后 会用选中项作为入参再次调用脚本
    4: 当没有输出时 整个过程结束
```

demo脚本:

编辑~/rofi.sh 以下内容并chmod +x rofi.sh

终端执行 `rofi -show powermenu -modi "powermenu:~/rofi.sh"`

```plaintext
case "$*" in
    "poweroff") 
        notify-send "Shutting down"
        echo yes
        echo no
        ;;
    "reboot")
        notify-send "reboot"
        ;;
    "lock")
        notify-send "lock"
        ;;

    "yes")
        notify-send "已触发关机"
        ;;
    "")
        echo poweroff
        echo reboot
        echo lock
        ;;
esac
```

## rofi dmenu模式的使用

[dmenu](https://github.com/davatorium/rofi/blob/next/doc/rofi-dmenu.5.markdown)

demo

```shell
file=$(ls | rofi -dmenu -window-title find)
echo $file
```

## 自用的rofi主题

config.rasi
```plaintext
configuration {
    theme: "mine.rasi";
}
```

mine.rasi
```plaintext
configuration {
  display-drun: "";
  display-window: "";
  display-windowcd: "";
  display-ssh: "";
  display-run: "﮸";
  show-icons: false;
  drun-display-format: "{icon} {name} {comment}";
}

* {
  font: "JetBrainsMono Nerd Font Mono 12.5";
  background-color: transparent;
  text-color: #f1f1f1;
  width: 680px;
  height: 300px;
  location: 0;
  spacing: 0;
  transparent: rgba(34,62,79,0.80);
}

window {
    location: center;
    anchor:   center;

    background-color:@transparent;
    spacing: 0;
    children:  [mainbox];
    orientation: horizontal;
}

inputbar {
  border: 0 0 1px 0;
  children: [prompt,entry];
}

prompt {
  padding: 0px 13px 0px 13px;
  border: 0 1px 0 0;
  vertical-align: 0.5;
}

textbox {
  background-color:@transparent;
  border: 0 0 1px 0;
  border-color: #161B1A;
  padding: 8px 13px;

}

entry {
  padding: 13px;
}

listview {
  cycle: false;
  margin: 0 0 -1px 0;
  scrollbar: false;
}

element {
  border: 0 0 1px 0;
  padding: 4px;
}

element-text {
  expand: true;
  vertical-align: 0.5;
  padding: 0px 13px;
}

element selected {
  background-color: rgba(34,82,99);
}

element-icon {
    size: 30px;
    border: 0px;
}
```
