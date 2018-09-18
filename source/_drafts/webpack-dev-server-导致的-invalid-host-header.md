---
title: webpack-dev-server 导致的 invalid host header
date: 2018-07-17 13:25:56
categories:
tags:
---
在 webpack-dev-server 的配置中添加
```
disableHostCheck: true
```

或者：
```
public: 'local.xxx.xxx'
```

webpack-dev-server 的配置是在 webpack.config.js 中的 devServer 字段。