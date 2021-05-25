---
title: VIM多光标插件——效率MAX
date: 2021-05-17 23:19:57
categories: VIM
tags: [VIM, NVIM]
excerpt: VIM多光标插件 —— vim-visual-multi 的配置和使用，以及日常工作中的应用
mermaid: false
---
<!-- markdown-toc GFM -->

* [插件简介 mg979/vim-visual-multi](#插件简介-mg979vim-visual-multi)
* [安装](#安装)
* [配置](#配置)
* [基本概念](#基本概念)
* [使用案例](#使用案例)
* [PS](#ps)

<!-- markdown-toc -->

## 插件简介 [mg979/vim-visual-multi](https://github.com/mg979/vim-visual-multi)

```plaintext
  VIM多光标插件，提供vim/nvim快速操作多光标编辑的能力
  本POST会简单介绍 其 安装、配置、日常使用的过程。
```

顺便在这里附上我自己的nvim配置仓库地址 [yaocccc/nvim](https://github.com/yaocccc/nvim)

## 安装

```plaintext
  推荐使用vim-plug安装

  Plug 'mg979/vim-visual-multi', {'branch': 'master'}
```

## 配置

```vimrc
  " 贴上本人的配置
      let g:VM_theme                      = 'ocean'
      let g:VM_highlight_matches          = 'underline'
      let g:VM_maps                       = {}
      let g:VM_maps['Find Under']         = '<C-n>'
      let g:VM_maps['Find Subword Under'] = '<C-n>'
      let g:VM_maps['Select All']         = '<C-d>'
      let g:VM_maps['Select h']           = '<C-Left>'
      let g:VM_maps['Select l']           = '<C-Right>'
      let g:VM_maps['Add Cursor Up']      = '<C-Up>'
      let g:VM_maps['Add Cursor Down']    = '<C-Down>'
      let g:VM_maps['Add Cursor At Pos']  = '<C-x>'
      let g:VM_maps['Add Cursor At Word'] = '<C-w>'
      let g:VM_maps['Remove Region']      = 'q'

  " 从上至下的意义:
      " ['Find Under']          -> 选中光标下的词(ctrl+n继续向下选中相同文本)
      " ['Find Subword Under']  -> 选中光标下的词(ctrl+n继续向下选中相同文本)
      " ['Select All']          -> 选中文件中所有光标下的词
      " ['Select h']            -> 从光标往左选中文本(ctrl+n继续向下选中相同文本)
      " ['Select l']            -> 从光标往右选中文本(ctrl+n继续向下选中相同文本)
      " ['Add Cursor Up']       -> 向上添加一个光标(原光标+上光标 继续使用则继续添加)
      " ['Add Cursor Down']     -> 向下添加一个光标(原光标+下光标 继续使用则继续添加)
      " ['Add Cursor At Pos']   -> 将当前光标添加入多光标列表中
      " ['Add Cursor At Word']  -> 将当前光标所在词的词首加上多光标列表中
      " ['Remove Region']       -> 移除当前光标
```

## 基本概念

**!!!重要!!!**
**!!!重要!!!**
**!!!重要!!!**

```plaintext
  简单来说该插件提供三种模式:
    多光标 normal 模式
    多光标 visual 模式
    多光标 insert 模式

  可以对应vim的三模式
  如何切换:
    normal <-> visual 模式互相转换: Tab
    normal --> insert 进入插入模式: i、a、c等 和 vim原生的按键是一样的
    insert --> normal 返回普通模式: Esc

  normal模式 可以使用 hjkl w e b f t $ ^ 0 等等按键来移动 多光标中的全部光标(不可以使用上下左右 PS: 上下左右可以用来移动原光标)
  visual模式 可以使用 hjkl w e b f t $ ^ 0 等等按键来选中 多光标中的全部光标(不可以使用上下左右 PS: 上下左右可以用来移动原光标)
  insert模式 可以使用 上下左右 等等按键来移动 多光标中的全部光标

  如何从正常VIM模式中进入多光标模式
    1: VIM处于normal模式
    2: 用以下快捷键即可
      ['Find Under']          -> 选中光标下的词(ctrl+n继续向下选中相同文本)
      ['Find Subword Under']  -> 选中光标下的词(ctrl+n继续向下选中相同文本)
      ['Select All']          -> 选中文件中所有光标下的词
      ['Select h']            -> 从光标往左选中文本(ctrl+n继续向下选中相同文本)
      ['Select l']            -> 从光标往右选中文本(ctrl+n继续向下选中相同文本)
      ['Add Cursor Up']       -> 向上添加一个光标(原光标+上光标 继续使用则继续添加)
      ['Add Cursor Down']     -> 向下添加一个光标(原光标+下光标 继续使用则继续添加)
      ['Add Cursor At Pos']   -> 将当前光标添加入多光标列表中
      ['Add Cursor At Word']  -> 将当前光标所在词的词首加上多光标列表中

  如何从多光标模式中退出
    一股脑Esc就对了
```

## 使用案例

```plaintext
  当前有以下文本，试完成以下案例 原始光标初始位置在第1行的l位置

  let aa = 1;
  let bbb = 2;
  let cccc = 3;
  let ddddd = 4;
```

```plaintext
  案例1: 将 全部let 改成 const
  案例重点: 如何添加多光标

  方案1: ctrl+d c const
    1: ctrl+d 选中文件中所有光标下的词
    2: c 从visual模式进入插入模式，并变更当前选中的文本
  方案2: ctrl+n ctrl+n ctrl+n ctrl+n c const
    1: ctrl+n 选中光标下的词 * 4次
    2: c 从visual模式进入插入模式，并变更当前选中的文本
  方案3: ctrl+下 ctrl+下 ctrl+下 xxx i const
    1: ctrl+下 * 3次 将共计4个l位置的光标添加进多光标列表
    2: xxx删除 i进入插入模式
  ...
```

![案例1](/img/vim多光标插件--效率MAX/0001.gif)

```plaintext
  案例2: 将 124行的 let 改成 const
  案例重点: 如何取消/跳过某光标

  方案1: ctrl+d 下下q c const
    1: ctrl+d 选中文件中所有光标下的词
    2: 下下 移动原始光标到第三行 q 取消当前光标
    3: c 从visual模式进入插入模式，并变更当前选中的文本
  方案2: ctrl+n ctrl+n 下下 ctrl+n c const
    1: ctrl+n 选中光标下的词 * 2次
    2: 下下 移动原始光标到第四行
    3: ctrl+n 选中光标下的词
    4: c 从visual模式进入插入模式，并变更当前选中的文本
  方案3: ctrl+下 下下 ctrl+x xxx i const
    1: ctrl+下 将共计2个l位置的光标添加进多光标列表
    2: 下下 移动原始光标到第四行
    3: ctrl+x 添加当前位置光标进多光标列表
    4: xxx删除 i进入插入模式
  ...
```

![案例2](/img/vim多光标插件--效率MAX/0002.gif)

```plaintext
  案例3: 将文本内容改成
  案例重点: 多光标下normal和visual的切换 光标的跳转等
    let aa = 1 * aa;
    let bbb = 2 * bbb;
    let cccc = 3 * cccc;
    let ddddd = 4 * ddddd;

  方案1: ctrl+d tab w tab e y tab ww a * 空格 esc p
    1: ctrl+d 选中文件中所有光标下的词
    2: tab 从visual模式转换到normal模式
    3: w 跳到 下个词首(aa、bbb等所在词)
    4: tab 从normal模式转换到visual模式
    5: e选中到词尾 y复制选中部分
    6: tab 从visual模式转换到normal模式
    7: ww a ... 光标跳转并插入 * 空格
    8: esc 从 insert模式回归到normal
    9: p粘贴刚刚选中的多个词
  方案2: ctrl+下 ctrl+下 ctrl+下 w tab e y tab ww a * 空格 esc p
    1: ctrl+下 * 3次 将共计4个l位置的光标添加进多光标列表
    2: w 跳到 下个词首(aa、bbb等所在词)
    3: tab 从normal模式转换到visual模式
    4: e选中到词尾 y复制选中部分
    5: tab 从visual模式转换到normal模式
    6: ww a ... 光标跳转并插入 * 空格
    7: esc 从 insert模式回归到normal
    8: p粘贴刚刚选中的多个词
  ...
```

![案例3](/img/vim多光标插件--效率MAX/0003.gif)

## PS

```plaintext
  建议将列出的按键配置都自己试一遍
  该插件还有不少其他用法，例如:
    自定义搜索文本跳转
    多行对齐
    ...

  不过我不会 有需要进阶的同好可以看原仓库说明
```

[mg979/vim-visual-multi](https://github.com/mg979/vim-visual-multi)
