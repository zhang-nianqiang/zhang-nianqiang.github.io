---
# PDF Start
# title: Typora自定义设置
# author: 张年强 
# subject: Workflow
# keywords: [Typora, Custom Settings]
# PDF End
# Github pages Jekyll Start
title: Typora自定义设置
author: 张年强
date: 2024-01-15 10:00:00 +0800
last_modified_at: 2024-01-17 10:00:00 +0800
categories: [Workflow]
tags: [Typora, Custom Settings]
image: 
  path: /githubpage/16511.01-00003-OD-WideTile.scale-400.png
  lqip: /githubpage/16511.01-00003-OD-WideTile.scale-400.png
# Github pages Jekyll End
---

## 前言

Typora作为一款优秀的Markdown编辑器,在使用的时候,进行自定义设置,符合自己的习惯和工作流,会让效率更高.

下面就介绍常用的的自定义内容.

---

## CSS

在 `C:\Users\xxx\AppData\Roaming\Typora\themes` 新建文件: `<主题名>.user.css` .

> [!NOTE] 
>
> ① 在这个文件内新增的设置会覆盖主题中CSS的设置.
>
> ② Typora升级时不会覆盖 `<主题名>.user.css` .

### 字体

当文件需要输出给其他人或组织时为避免版权风险,使用开源可商用字体.

1. 编辑页面

```css
body {
  font-family: "Fira Code Medium", "思源黑体 CN Medium";
}
```

2. 代码块

```css
body {
  --monospace: "Fira Code Medium", "思源黑体 CN Medium";
}
```

3. 字体大小

文件 -> 偏好设置 -> 外观 -> 字体大小 18px

### 分页符

在导出PDF时需要使用分页符来控制排版,可以使用连续三个 `-` 来实现分页

```css
@media print, (overflow-block: paged) or (overflow-block: optional-paged)
{
  hr
  {
    page-break-after: always; /* CSS 2 */
         break-after: region; /* CSS 3+ */
    /* minimal layout disruption: */
    height: 0.1mm; visibility: hidden;
  }
}
```

### 标题间距

更改`margin-top` `margin-bottom` 的数值,使标题间距增大或缩小.

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

## 搜索引擎

打开 `C:\Users\xxx\AppData\Roaming\Typora\conf\conf.user.json` ,由于Google在国内访问困难,更改为Bing.

```json
  "searchService": [
    ["Search with Bing", "https://cn.bing.com/search?q=%s"]
  ]
```

## 图片上传

图床使用的是[缤纷云](https://www.bitiful.com),使用了[bitiful-typora-upload-cli](https://github.com/XRSec/bitiful-typora-upload-cli)插件,感谢[XRSec](https://github.com/XRSec)的工作!

1. 设置缤纷云参数

   登录 -> 对象存储 

   ① 桶列表 -> 桶信息 -记录其中的Endpoint/Region

   ② AccessKey -> 添加子用户 -> 设定权限 全选

   ③ AccessKey -> 添加Key -> 记录AccessKeyID/AccessKeySecret

2. 设置图床参数

   新建 `.txt` 文件,复制并填写以下信息.

   ```yaml
   Endpoint: s3.bitiful.net
   Region: cn-east-1
   AccessKeyID: "xxxxxxxxxxx"
   AccessKeySecret: "xxxxxxxxxxxxxxxx"
   BucketName: "xxxxxxxxxx"
   Path: "/xxxxxxxx/"
   ```

   另存为: `bitiful.yml` 存放在C:\Users\xxx\AppData\Roaming文件夹.

3. 下载[bitiful-typora-upload-cli](https://github.com/XRSec/bitiful-typora-upload-cli/releases)插件 `bitiful-windows-386.exe`

4. 设置上传方式

   文件 -> 偏好设置 -> 图像 -> 上传服务设定 -> 自定义命令

   复制exe的路径,例如: `C:\Users\XXX\AppData\Roaming\bitiful\bitiful-windows-386.exe` 粘贴在自定义命令上.

5. 验证图片上传

   文件 -> 偏好设置 -> 图像 -> 上传服务设定 -> 点击 `验证图片上传选项`

   成功会有弹窗提示.

6. 测试图片上传自动替换网络地址

> [!NOTE]  
>
> ① 上传后会删除本地图片,如想保留可以:
>
> 文件 -> 偏好设置 -> 图像 -> 插入图片时 -> 复制图片.../选择对本地图片
>
> ② 上传后的图片文件名是日期格式.

---
