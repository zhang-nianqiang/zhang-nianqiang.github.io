---
# PDF Start
# title: BookStack 自定义配置
# author: 张年强
# subject: BookStack
# keywords: [自定义配置, BookStack]
# PDF End
# Obsidian/Jekyll Start
title: BookStack 自定义配置
author: 张年强
date: 2025-07-03
last_modified_at: 2025-12-14
categories: [BookStack]
tags: [自定义配置, BookStack]
description: 通过 CSS 的方法调整 BookStack 的页面样式及 S3 存储相关的设定方法
# Obsidian/Jekyll End
---

## 1 样式调整

登录 admin->设置->个性化->自定义 HTML 头部内容

### 1.1 更换字体

更改字体，需要使用以下代码。

以下引入的是通过文风字体提供的抖音美好体及 MiSans。

```css
<link rel='preconnect' href='https://cn.windfonts.com' crossorigin>
<link rel="stylesheet" crossorigin="anonymous" href="https://app.windfonts.com/api/css?family=wenfeng-dymht:wght@regular">
<!-- 此中文网页字体由文风字体（Windfonts）免费提供，您可以自由引用，请务必保留此授权许可标注 https://wenfeng.org/license -->
<link rel='preconnect' href='https://cn.windfonts.com' crossorigin>
<link rel="stylesheet" crossorigin="anonymous" href="https://app.windfonts.com/api/css?family=wenfeng-misa:wght@medium">
<!-- 此中文网页字体由文风字体（Windfonts）免费提供，您可以自由引用，请务必保留此授权许可标注 https://wenfeng.org/license -->
<style>
   body, button, input, select, label, textarea {
       --font-body: "wenfeng-misa", 'Noto Serif', serif;
       --font-heading: "wenfeng-dymht", 'Roboto', sans-serif;
       --font-code: 'Source Code Pro', monospace;
  }
 </script>
```

> [!NOTE]
>
> 请注意，这个方法引入字体虽然简单，但由于网络原因，网页加载速度变慢。

### 1.2 调整字体

```css
<style>
  h1 {
    display: block;
    font-size: 3em;
    line-height: 200%;
  }
  h2 {
    display: block;
    font-size: 2.5em;
    line-height: 200%;
  }
  h3 {
    display: block;
    font-size: 2em;
    line-height: 200%;
  }
  h4 {
    display: block;
    font-size: 2em;
    line-height: 200%;
  }
 li {
    padding: 1px;
    line-height: 1.8em;
    font-size: 18px;
  }
  p {
    margin-top: 5px;
    margin-bottom: 5px;
    font-size: 18px;
  }
  p img {
    margin-top: 5px;
    margin-bottom: 5px;
  }
  li p {
    margin-top: 5px;
  }
</style>
```

### 1.3 页面样式

页面的摘要不显示，需要使用以下代码。

```css
<style>
  .page .text-muted {
    display: none !important;
}
</style>
```

### 1.4 章节样式

章节内的页面在显示时默认显示，需要使用一下代码。

```html
<script type="text/javascript">
  window.addEventListener("DOMContentLoaded", (event) => {
    window.$components.get("chapter-contents").forEach((toggle) => {
      toggle.open();
      toggle.isOpen = true;
    });
  });
</script>
```

## 2 合规

登录 admin->设置->个性化->页脚链接

增加 ICP 备案及公安联网备案。

## 3 S3 存储

基于一些原因，选择网站内用户上传的图片及文件存储在网站运行服务器之外，此时 S3 存储是一个比较好的选择，并且被 BookStack 原生支持。

基于一些原因，选择缤纷云作为 S3 存储。

### 3.1 缤纷云中设置

1. 登录缤纷云，新建存储桶。

2. 记录并存储存包括：储桶名称；Secret Key； Access Key 及 Endpoint。

> [!CAUTION]
>
> Secret Key 只会在新建存储桶时显示一次。

### 3.2 BookStack 中设置

1. 登录网站服务器，将目录更改为/var/www/BookStack ：

```shell
cd /var/www/BookStack/
```

2. 打开.env 文件：

```shell
sudo nano .env
```

3. 在 `.env` 中增加：

```ini
STORAGE_TYPE=s3
STORAGE_S3_KEY=缤纷云-Access Key
STORAGE_S3_SECRET=缤纷云-Secret Key
STORAGE_S3_BUCKET=缤纷云-存储桶名称
STORAGE_S3_ENDPOINT=https://缤纷云-Endpoint
STORAGE_URL=https://缤纷云-存储桶名称.缤纷云-Endpoint/
```

这些设置会使 BookStack 在你的缤纷云存储库中存储图片及附件。

4. 保存，退出。

## 4 CDN 加速

基于一些安全及其他原因，缤纷云对图片的原始存储地址打开方式进行限制：强制下载。这导致在 BookStack 点击图片时就会下载图片。

一般而言，这不是网站期望的点击图片的方式。

当然，缤纷云对使用 CND 的方式不做限制。所以接下来使用 CDN 的方式处理图片。

### 4.1 缤纷云中设置

登录缤纷云，新建 CND 加速。

| 域名地址    | 源站 | CNAME 域名          |
| ----------- | ---- | ------------------- |
| cdn.xxx.com |      | cdn.xxx.com.xxx.com |

> [!CAUTION]
>
> 加速域名不能使用根域名，可以使用类似： cdn.xxx.com。

### 4.2 解析 CDN 加速域名

登录域名服务商，进入域名管理界面，增加解析。

| 主机记录 | 记录类型 | 记录值              |
| :------- | :------- | :------------------ |
| cdn      | CNAME    | cdn.xxx.com.xxx.com |

### 4.3 BookStack 中设置

1. 登录网站服务器，将目录更改为/var/www/BookStack ：

```shell
cd /var/www/BookStack/
```

2. 打开.env 文件：

```shell
sudo nano .env
```

3. 在 `.env` 中变更：

```ini
STORAGE_URL=http://cdn.xxx.com
```

4. 保存，退出

## 5 自动证书

当需要使用 <https://cdn.xxx.com> 的形式访问 CDN 加速域名时需要使用证书。

### 5.1 缤纷云中设置

- 在证书中心里申请自动证书。
- 在 CDN 加速里点击管理，启用 HTTPS。

### 5.2 解析 CDN 加速域名

进入域名管理界面，增加解析。

| 主机记录             | 记录类型 | 记录值                  |
| -------------------- | -------- | ----------------------- |
| \_acme-challenge.cdn | CNAME    | xxx.btf.cnamecenter.com |

### 5.3 BookStack 中设置

在 `.env` 中变更：

```ini
STORAGE_URL=https://cdn.xxx.com
```
