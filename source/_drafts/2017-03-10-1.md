---
title: UILabel文字自然对齐
date: 2017-03-10 21:59:01
categories: iOS
tags:
---

#### 前言
今天遇到一个需求，当UILabel的内容只有一行的时候文字右对齐，内容多行的时候最后一行左对齐(对判断文字超出一行将UILabel改为左对齐的方法say no)。<!--more-->

#### 解决思路
在NSTextAlignment枚举里有个叫NSTextAlignmentJustified（其解释如下Fully-justified. The last line in a paragraph is natural-aligned.）可以实现最后一行自然对齐。

``` objc 

    UILabel * label = [[UILabel alloc] init];

    label.numberOfLines = 0;

    //先设置文字居右对齐
    label.textAlignment = NSTextAlignmentRight;

    //再设置最后一行自然对齐
    label.textAlignment = NSTextAlignmentJustified;

```

如下图所示
1、第一行 居右对齐
2、第二行 居右对齐且换行
3、第三行 居右对齐且换行且最后一行自然对齐（即我们要实现的效果）

![](http://ojgg6fpio.bkt.clouddn.com/Simulator%20Screen%20Shot%202017%E5%B9%B43%E6%9C%8810%E6%97%A5%20%E4%B8%8B%E5%8D%8810.10.43.png)


# 感谢 曾俊洲同学提供解决方案
