---
# PDF Start
# title: 解决 Minimal Mistakes Theme 浏览器标签页图标不显示
# author: 张年强
# subject: Jekyll
# keywords: [Minimal Mistakes Theme, Jekyll, 标签页图标]
# PDF End
# Obsidian/Jekyll Start
title: 解决 Minimal Mistakes Theme 浏览器标签页图标不显示
author: 张年强
date: 2026-03-17
last_modified_at: 2026-03-17
categories: [Jekyll]
tags: [Minimal Mistakes Theme, Jekyll, 标签页图标]
# Obsidian/Jekyll End
---

## 1. 故障定义

### 1.1 故障现象

在应用 Minimal Mistakes Theme 后，浏览器标签页图标显示的不是预期的自定义图标，而是默认图标或上一个使用的主题标签页图标。

### 1.2 故障原理

#### 1.2.1 原因一

浏览器未重新加载新图标，或者说浏览器的标签页图标缓存没有刷新。

#### 1.2.2 原因二

标签页图标配置错误。

在 Minimal Mistakes Theme 中，标签页图标是按照浏览器的默认方式（例如：网站根目录查找 `favicon.ico` 文件）进行加载，并没有`favicon.ico` 配置项。

## 2. 解决方案

### 2.1 检查配置方式

确认你的 favicon.ico 放置的位置正确，如下显示位置：

```
你的网站根目录/
├── _config.yml
├── favicon.ico          ← favicon 文件放在这里
├── _includes/
├── _layouts/
├── _pages/
├── _posts/
└── assets/
```

### 2.2 检查文件部署正确

访问 `https://你的域名.com/favicon.ico`，能够正常显示你需要的图片，则说明部署正确。

### 2.3 强制刷新缓存

**Windows:** 打开你的网站，然后 `Ctrl + Shift + R` 。
