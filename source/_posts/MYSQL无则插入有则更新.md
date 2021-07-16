---
title: MYSQL无则插入有则更新
date: 2021-05-17 10:43:24
categories: MYSQL
tags: MYSQL
excerpt: MYSQL语法实现需求, 唯一索引冲突时更新，不冲突时插入
---

<!-- markdown-toc GitLab -->

* [需求分析](#需求分析)
* [直接上结论](#直接上结论)

<!-- markdown-toc -->

## 需求分析

```plaintext
  用一条sql实现
  数据库中已存在(主键或唯一索引已存在)则更新原来的数据
  数据库中不存在(主键或唯一索引为新值)则插入一条新数据
```

## 直接上结论

```sql
INSERT INTO TABLENAME …… ON DUPLICATE KEY UPDATE ……
PS: 该语法为MYSQL特有语法，不是SQL标准语法
```
