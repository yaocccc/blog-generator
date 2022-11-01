---
title: golang中for range的坑点
date: 2021-10-15 16:48:43
tags: [GOLANG]
excerpt: 记录golang中for range的坑点
---

<!-- markdown-toc GitLab -->

* [golang中for range的坑点](#golang中for-range的坑点)
  * [错误例子](#错误例子)
  * [原因分析](#原因分析)
  * [正确处理](#正确处理)

<!-- markdown-toc -->

## golang中for range的坑点

GOLANG 可通过 for range 遍历 数组、切片、映射等类型。

但使用不当时 有时程序运行会不符合预期。

### 错误例子

```golang
package main

import "fmt"

func main() {
    sourceArr := []int{0, 1, 2, 3}
    targetArr := []*int{}

    for _, num := range sourceArr {
        targetArr = append(targetArr, &num)
    }

    for _, num_p := range targetArr {
        fmt.Printf("num: %v\n", *num_p)
    }
}
```

```plaintext
# 预期结果为
num: 0
num: 1
num: 2
num: 3

# 实际结果为
num: 3
num: 3
num: 3
num: 3
```

### 原因分析

进一步分析打印程序如下 打印targetArr如下

```golang
package main

import "fmt"

func main() {
    sourceArr := []int{0, 1, 2, 3}
    targetArr := []*int{}

    for _, num := range sourceArr {
        fmt.Printf("&num: %+v\n", &num)
        targetArr = append(targetArr, &num)
    }

    for _, num_p := range targetArr {
        fmt.Printf("num: %v\n", *num_p)
    }
    fmt.Printf("targetArr: %v\n", targetArr)
}
```

```plaintext
# 结果如下
&num: 0xc00018c000
&num: 0xc00018c000
&num: 0xc00018c000
&num: 0xc00018c000
num: 3
num: 3
num: 3
num: 3
targetArr: [0xc00018c000 0xc00018c000 0xc00018c000 0xc00018c000]

可见 range 的第二返回值 始终指向同一个地址
导致 *num_p 取得的都是 sourceArr 的最后一项

!!! 一旦在for range中 使用了 第二返回值 & 取值赋值时，都要谨慎处理
```

### 正确处理

```golang
package main

import "fmt"

func main() {
    sourceArr := []int{0, 1, 2, 3}
    targetArr := []*int{}

    for idx := range sourceArr {
        targetArr = append(targetArr, &sourceArr[idx])
    }

    for _, num_p := range targetArr {
        fmt.Printf("num: %v\n", *num_p)
    }
}
```

当存在指针传递时，尽量使用index作为赋值依据，上述程序 符合预期
