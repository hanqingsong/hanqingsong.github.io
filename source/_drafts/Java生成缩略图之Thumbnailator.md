---
title: Java生成缩略图之Thumbnailator
date: 2018-04-08 18:47:22
categories: java
tags: java
---
## 介绍
Thumbnailator是个开源的Java 项目，它提供了非常简单流畅的 API 来对图片进行缩放、旋转、批量处理图片以及加水印的处理。

## 使用
```
Thumbnails.of(file.getInputStream()).scale(1.0).rotate(0)
                    .outputQuality(0.5D).imageType(BufferedImage.TYPE_INT_ARGB)
                    .toOutputStream(outputStream);
```

## 资料
https://item.congci.com/-/content/thumbnailator-tupian-suofang-xuanzhuan-jia-shuiyin