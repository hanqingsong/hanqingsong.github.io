---
title: 记一次npm i 运行错误
date: 2017-09-20 15:24:46
categories: node
tags: node
---

执行npm i出错:

```
npm ERR! code EINTEGRITY
npm ERR! sha1-eCA6TRwyiuHYbcpkYONptX9AVa4= integrity checksum failed when using sha1: wanted sha1-eCA6TRwyiuHYbcpkYONptX9AVa4= but got sha1-tURkGu3SzDOsTOBkGT5fXZCH7as=. (26893 bytes)

npm ERR! A complete log of this run can be found in:
```

解决方法：npm cache clean --force  强制清除npm 的 cache
或者重新执行 npm i