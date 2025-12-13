---
# PDF Start
# title: Typora 自定义配置
# author: 张年强
# subject: Text
# keywords: [Typora, 自定义配置]
# PDF End
# Obsidian/Jekyll Start
title: Typora 自定义配置
author: 张年强
date: 2024-01-15
last_modified_at: 2025-12-10
categories: [Text]
tags: [Typora, 自定义配置]
description: 以新增用户 CSS 的方法调整页面样式及导出条自定义方法
# Obsidian/Jekyll End
---

## 1 CSS

Typora 覆盖安装升级时会将自带的 theme 文件重置为默认，原来使用的 theme 文件备份为 Old，自定义主题使用以下官方推荐的方法会减少工作量。

打开 `C:\Users\xxx\AppData\Roaming\Typora\themes` 或使用：文件 -> 偏好设置 -> 外观 -> 打开主题文件夹，新建文件：`<主题名>.user.css` 。

> [!NOTE]
>
> ① 在这个文件内新增的设置会覆盖主题中 CSS 的设置。
>
> ② Typora 升级时不会覆盖 `<主题名>.user.css` 。
>
> ③ `AppData`文件夹默认是隐藏文件夹，想要显示此文件夹：文件夹选项->选择“显示隐藏文件夹”。

### 1.1 字体

当文件需要展示或输出给其他人或组织时，为避免版权风险使用开源可商用字体。

由于很多软件无法针对中文和英文单独设置字体，出于统一工作流的考虑，只设置一种字体。

#### 1.1.1 编辑页面

```css
body {
  font-family: "梦源黑体 CN W16";
}
```

#### 1.1.2 代码块

```css
body {
  --monospace: "梦源黑体 CN W16";
}
```

#### 1.1.3 字体大小

- 全局字体大小 文件 -> 偏好设置 -> 外观 -> 字体大小 16px。
- 表格字体大小：

```css
table {
  font-size: 0.75em;
}
```

### 1.2 分页符

在导出 PDF 时需要使用分页符来进行简单排版。

#### 1.2.1 方法一

在正文中需要分页的地方输入连续三个 `-` 来实现分页。

在 CSS 中增加以下代码：

```css
@media print, (overflow-block: paged) or (overflow-block: optional-paged) {
  hr {
    page-break-after: always; /* CSS 2 */
    break-after: region; /* CSS 3+ */
    /* minimal layout disruption: */
    height: 0.1mm;
    visibility: hidden;
  }
}
```

> [!NOTE]
>
> 注意：使用此方法进行分页时，如果同时使用 footnote，footnote 将出现在整个文章最后的单独一页。

#### 1.2.2 方法二

在正文中需要分页的地方输入以下代码：

```html
<div class="page-break"></div>
```

在 CSS 中增加以下代码：

```css
.page-break {
  display: none;
}
@media print, (overflow-block: paged) or (overflow-block: optional-paged) {
  .page-break {
    display: block;
    page-break-after: always; /* CSS 2 */
    break-after: page; /* CSS 3+ */
  }
}
```

> [!NOTE]
>
> 注意：使用此方法进行分页时，如果同时使用 footnote，footnote 将出现在整个文章的最后。

### 1.3 标题

#### 1.3.1 标题下划线

h1 及 h2 会有一个下划线，如果想取消下划线，在 CSS 中增加以下代码：

```css
h1,
h2 {
  border-bottom: none;
}
```

#### 1.3.2 标题间距

为适应个人的标题间距喜好,更改`margin-top` `margin-bottom` 的数值，使标题间距增大或缩小。

```css
h1,
h2,
h3,
h4,
h5,
h6 {
  position: relative;
  margin-top: 2rem;
  margin-bottom: 2rem;
  font-weight: bold;
  line-height: 1.4;
  cursor: text;
}
```

### 1.4 书写区域宽度

为使书写时和导出的 PDF 显示一样，修改为以下的数值。

```css
#write {
  max-width: 661px; /*adjust writing area position*/
}
```

### 1.5 表格样式

为适应复杂表格，表格内字与边框的间距缩小到以下数值：

```css
table th {
  padding: 6px 6px;
}
table td {
  padding: 6px 6px;
}
```

## 2 搜索引擎

由于 Google 在国内访问困难，更改为 Bing。

打开 `C:\Users\xxx\AppData\Roaming\Typora\conf\conf.user.json` 按照下面进行修改。

```json
  "searchService": [
    ["Search with Bing", "https://cn.bing.com/search?q=%s"]
  ]
```

## 3 图片

Markdown 格式文件一般并不保存图片在文件内，但由于Typora是实时渲染，为了保障性能，正文中的图片适合使用图床。

而本地图片不在一个文件夹内管理所有图片。

工作流程如下：

- 文档编辑。
- 图片编辑。
- 使用图床的网页进行整体图片上传。
- 文档进行图片的插入。

虽然流程繁复了一些，但不需要依赖其他软件，适合整体的工作流。

## 4 PDF 导出

文件 -> 偏好设置 -> 导出 -> PDF ->页首（${title}）。

文件 -> 偏好设置 -> 导出 -> PDF ->页尾（No. ${pageNo} / ${totalPages}）。
