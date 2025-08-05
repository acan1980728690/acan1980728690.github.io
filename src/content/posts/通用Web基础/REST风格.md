---
title: REST风格
published: 2025-08-01 19:50:32
tags: []
category: 通用web基础
draft: false
---

REST（Representational State Transfer），表述性状态转换，它是一种软件架构风格

示例

```
    http://localhost:8080/users/1       GET：查询id为1的用户
    http://localhost:8080/users          POST：新增用户
    http://localhost:8080/users          PUT：修改用户
    http://localhost:8080/users/1       DELETE：删除id为1的用户
```

其中总结起来，就一句话：通过URL定位要操作的资源，通过HTTP动词(请求方式)来描述具体的操作。

REST风格的URL中，通过四种请求方式，来操作数据增删改查

- GET ：  查询
- POST ：新增
- PUT ：  修改
- DELETE ：删除