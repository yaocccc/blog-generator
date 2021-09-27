---
title: 也许是最便捷的SS客户端
date: 2021-09-27 10:22:21
categories: SS
tags: [LINUX, SS]
excerpt: 利用v2raya翻山越海
mermaid: false
---

<!-- markdown-toc GitLab -->

* [利用docker启动](#利用docker启动)
* [配置](#配置)
* [V2RAY配置情况](#v2ray配置情况)

<!-- markdown-toc -->

## 利用docker启动

```shell
  docker run -d \
    --restart=always \
    --privileged \
    --network=host \
    --name v2raya \
    -e V2RAYA_ADDRESS=0.0.0.0:2017 \
    -v /lib/modules:/lib/modules \
    -v /etc/resolv.conf:/etc/resolv.conf \
    -v /etc/v2raya:/etc/v2raya \
    mzz2017/v2raya
```

## 配置

```plaintext
  从浏览器访问 localhost:2017
  然后从页面上配置即可
```

## V2RAY配置情况

![V2RAY配置情况](/img/也许是最便捷的SS客户端/001.png)
