---
title: 写一个可以键盘控制的shell脚本
date: 2021-10-19 18:47:54
categories: LINUX
tags: [LINUX, SHELL]
excerpt: 写一个可以用键盘上下左右控制的 带菜单选择的脚本
---

<!-- markdown-toc GitLab -->

* [写一个可以键盘控制的shell脚本](#写一个可以键盘控制的shell脚本)
  * [MENU脚本](#menu脚本)
    * [MENU脚本讲解](#menu脚本讲解)
  * [DEMO脚本](#demo脚本)
  * [效果展示](#效果展示)
  * [一些示范脚本](#一些示范脚本)

<!-- markdown-toc -->

## 写一个可以键盘控制的shell脚本

### MENU脚本

[menu脚本文件 可以直接保存到本地](/file/menu)

若无法下载 则手动编辑本文文件

!!!PS!!!  
menu()中的 ^C ^M ^[ 分别代表ctrl+c 回车 esc 且不能直接用字符形式输入  
输入方法为  
在vim或vi插入模式中(ctrl+v会触发源字符输入模式)  
^C: ctrl-v ctrl-c  
^M: ctrl-v enter  
^[: ctrl-v esc


```shell
# menu
key=''
_echo_green() { echo -e "\033[32m$1\033[0m"; }
_get_char() { SAVEDSTTY=`stty -g`; stty -echo; stty raw; dd if=/dev/tty bs=1 count=1 2> /dev/null; stty -raw; stty echo; stty $SAVEDSTTY; }
_list() {
    text=''
    for tab in ${menu_tabs[@]}; do
        test ${tab} = ${menu_tabs[$tab_index]} && text=$text' \033[32m'$tab'\033[0m'  || text=$text' '$tab
    done
    echo -e $text

    for item in ${menu_items[@]}; do
        test ${item} = ${menu_items[$item_index]} && _echo_green " > ${item}" || echo "   ${item}"
    done
}
_key() {
    # 计算新的tab_index
    tab_index=$(($tab_index+$1))
    len=${#menu_tabs[*]}
    test $tab_index -lt 0 && tab_index=$((len - 1))
    test $tab_index -gt $((len - 1)) && tab_index=0

    # 计算新的item_index
    item_index=$(($item_index+$2))
    len=${#menu_items[*]}
    test $item_index -lt 0 && item_index=$((len - 1))
    test $item_index -gt $((len - 1)) && item_index=0

    clear

    pre_hook
    _list
    after_hook
}

###############################################

function pre_hook() { :; }
function after_hook() { :; }
menu_tabs=()
menu_items=()

# 调用menu方法展开菜单
# 上下左右移动tab或item，回车选中 q Q ctrl-c 退出脚本
menu() {
    _key 0 0
    while :; do
        key=`_get_char`
        case "$key" in
            'q'|'Q'|'^C') exit 1 ;;
            '^M') break ;;
            '^[')
                secondchar=`_get_char`
                thirdchar=`_get_char`
                case "$thirdchar" in
                    A) _key 0 -1 ;;
                    B) _key 0 1 ;;
                    D) _key -1 0 ;;
                    C) _key 1 0 ;;
                esac ;;
        esac
    done
}
```

#### MENU脚本讲解

```plaintext
# 用到的依赖func，这些不允许用户自定义或主动调用
_echo_green(): 用于打印绿色文本
_get_char(): 用于从键盘获取操作
_list(): 渲染菜单
_key(): 计算新的tab_index、item_index并渲染菜单的func

# 用户可自定义的变量和func
menu_tabs: 用于自定义tab项 列表 例如 (1 2 3 4)
menu_items: 用于自定义当前的item项 列表 例如 ('item1' 'item2' 'item3')
pre_hook(): 发生在渲染菜单前的钩子方法(此时新的tab_index、item_index已计算完成)
after_hook(): 发生在渲染菜单后的钩子方法

# 用户可使用的变量和func
tab_index: 当前的tab索引号 从0开始
item_index: 当前的item索引号 从0开始
menu(): 进入菜单选择状态的入口func
```

### DEMO脚本

```shell
    source ./menu
    menu_tabs=('tab1' 'tab2' 'tab3')
    menu_items=('item1' 'item2' 'item3')
    pre_hook() {
        echo '请选择tab or item:'
    }
    after_hook() {
        echo '当前选中项为:' ${menu_tabs[$tab_index]} ${menu_items[$item_index]}
    }

    # 调用 func: menu 开始菜单 [上下左右移动] [回车选中] [q或esc或ctrl c结束]
    menu
    echo 结束了
    echo 最终选中的项为: ${menu_tabs[$tab_index]} ${menu_items[$item_index]}
```

### 效果展示

![show](/img/如何写一个可以键盘控制的shell脚本/001.gif)

### 一些示范脚本

快速连接远程服务器  

./ssh.sh ls 开始菜单选择对应命令执行  
./ssh.sh \*非ls 直接执行 ssh *

```shell
#!/bin/bash

source ./menu

menu_items=("跳板机" "腾讯云" "跳板机2" "跳板机win" "翻墙机" "公网机")

cmds[0]='ssh **@a.b.com'
cmds[1]='ssh root@host1'
cmds[2]='sshpass -p ****** ssh **@jms.hwwt2.com -p 2223'
cmds[3]='rdesktop -u ******** -p ****** ip:3390 -r sound:off -g 1920x1080'
cmds[4]='ssh root@host2'
cmds[5]='ssh root@255.255.255.251'

after_hook() {
    echo
    echo '   '${cmds[$item_index]}
}

case $1 in
    ls)
        menu
        echo 连接${menu_items[$item_index]}
        exec ${cmds[$item_index]}
        ;;
    *) ssh $*;;
esac
```
