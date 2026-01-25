---
# PDF Start
# title: 解决 Minimal Mistakes Theme 搜索功能失效
# author: 张年强
# subject: Jekyll
# keywords: [Minimal Mistakes Theme, Jekyll Gfm Admonitions]
# PDF End
# Obsidian/Jekyll Start
title: 解决 Minimal Mistakes Theme 搜索功能失效
author: 张年强
date: 2026-01-23
last_modified_at: 2026-01-23
categories: [Jekyll]
tags: [Minimal Mistakes Theme, Jekyll Gfm Admonitions]
description: 解决 Minimal Mistakes Theme 搜索功能与 Jekyll Gfm Admonitions 冲突的问题
# Obsidian/Jekyll End
---

## 1. 故障定义

### 1.1 故障现象

Minimal Mistakes Theme启用Jekyll Gfm Admonitions插件后，搜索功能失效。

点击搜索按钮，输入搜索关键词后，没有搜索结果显示。

### 1.2 故障原理

此故障发生在Minimal Mistakes Theme使用Lunr搜索引擎。

`lunr-en.js`文件的头部被Jekyll Gfm Admonitions注入了以下代码：

```html
<head>
  <style>
    .......
  </style>
</head>
```

导致`lunr-en.js`文件没有正常调用，搜索没有结果显示。

`lunr-en.js`能够被注入的原因是其开头有YAML内容被Jekyll Gfm Admonitions认为其是文档文件。

## 2. 解决方案

删除`lunr-en.js`前三行
