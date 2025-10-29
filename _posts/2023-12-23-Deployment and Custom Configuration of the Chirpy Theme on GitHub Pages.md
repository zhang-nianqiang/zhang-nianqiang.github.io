---
# PDF Start
# title: Chirpy Theme 在 GitHub Pages 部署及自定义配置
# author: 张年强
# subject: Jekyll
# keywords: [Chirpy_Theme, Jekyll, GitHub_Pages]
# PDF End
# Obsidian/Jekyll Start
title: Chirpy Theme 在 GitHub Pages 部署及自定义配置
author: 张年强
date: 2023-12-23
last_modified_at: 2025-10-29
categories: [Jekyll]
tags: [Chirpy_Theme, Jekyll, GitHub_Pages]
# Obsidian/Jekyll End
---

## 1 部署

在 GitHub Pages 上使用`主题模板`部署的方法。

### 1.1 新建仓库

打开[Chirpy Start](https://github.com/cotes2020/chirpy-starter/generate) -> 键入仓库名称。

> [!IMPORTANT]
>
> 仓库名使用：<用户名.github.io>。

### 1.2 配置 GitHub 仓库

选择 GitHub Actions 为构建和部署源。

_GitHub.com -> Repository -> Settings -> Pages -> Build and deployment -> Source -> GitHub Actions_

---

## 2 设置 Chirpy Theme

### 2.1 基础设置

打开 `_config.yml` ：

```yaml
timezone: Asia/Shanghai
title: XXX
tagline: XXX
description: >-
  XXX
url: "https://用户名.github.io"
github:
  username: XXX
social:
  name: XXX
  email: XXX@XXX.com
  links:
    - https://github.com/用户名
```

> [!IMPORTANT]
>
> `url` 不能以 `/` 结尾。

### 2.2 使用图片 CDN 功能

当使用图库时可以使用图片 CDN 功能,可以减少图片链接的固定输入工作。

打开 `_config.yml` ：

```yaml
cdn: https://XXX.com
avatar: "/XXX.jpg"
```

> [!IMPORTANT]
>
> 图片链接使用时以 `/` 开头。

### 2.3 使用 Giscus 增加评论功能

方法一：使用与博客相同的仓库增加评论功能。

方法二：使用与博客不同的仓库增加评论功能。

作为非专业人士，选择方法二。

这样选择的好处是：

- 博客仓库的变更（包括更换主题及崩溃后重新部署），不影响评论数据的安全。

  > [!IMPORTANT]
  >
  > 博客仓库名及文章名不能变更。

- 博客主题升级时可以采用`主题模板`方法重新部署，不需要本地编译环境。

  > [!IMPORTANT]
  >
  > 升级前需要仓库整体备份，升级后修改变更部分。

新建仓库后按以下进行操作：

1. 选择使用 Discussions。

   _GitHub.com -> Repository -> Settings -> General -> Features -> Discussions_

2. 安装 [giscus app](https://github.com/apps/giscus) 到仓库。

3. 访问 [giscus](https://giscus.app/zh-CN) 设置。

4. 填写<用户名/仓库名>并选择参数。

5. 按照提示在 `_config.yml` 填写：

   ```yaml
   comments:
     provider: "giscus"
     giscus:
       repo: 用户名/仓库名
       repo_id: XXXXXXXX
       category: Announcements
       category_id: XXXXXXXX
       mapping: "pathname"
       strict: 0
       input_position: "top"
       lang: zh-CN
       reactions_enabled: 1
   ```

   > [!IMPORTANT]
   >
   > ① 评论设置中的 `lang` 设置需要和本地化的 `lang` 一致。
   >
   > ② 所填内容与 `#` 需要有空格间隔。
   >
   > ③ 不同设置需要不同符号，单引号或双引号或无符号。

### 2.4 本地化

1. 打开 `_config.yml` ：

   ```yaml
   lang: zh-CN
   ```

2. 在 [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 的 `_data/locales/` 复制 `zh-CN.yml` 文件到仓库相同文件夹上传。

### 2.5 版权声明

打开 `_data/locales/zh-CN.yml` ：

```yaml
copyright:
  # Shown at the bottom of the post
  license:
    template: 本文由作者按照 :LICENSE_NAME 进行授权
    name: CC BY 4.0
    link: https://creativecommons.org/licenses/by/4.0/

  # Displayed in the footer
  brief: 保留部分权利。
  verbose: >-
    除非另有说明，本网站上的博客文章均由作者按照知识共享署名 4.0 国际 (CC BY 4.0) 许可协议进行授权。
```

按照需要修改。

### 2.6 删除社交图标

打开 `_data/contact.yml` ,在需要删除的社交图标前增加 `#` ：

```yaml
# - type: twitter
#   icon: "fa-brands fa-x-twitter"
```

### 2.7 删除分享图标

打开 `_data/share.yml` ,在需要删除的分享图标前增加 `#` ：

```yaml
platforms:
#  - type: Twitter
#    icon: "fa-brands fa-square-x-twitter"
#    link: "https://twitter.com/intent/tweet?text=TITLE&url=URL"

#  - type: Facebook
#    icon: "fab fa-facebook-square"
#    link: "https://www.facebook.com/sharer/sharer.php?title=TITLE&u=URL"

#  - type: Telegram
#    icon: "fab fa-telegram"
#    link: "https://t.me/share/url?url=URL&text=TITLE"
```

### 2.8 使用子模块

打开 `.github/workflows/pages-deploy.yml` ：

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4
    with:
      submodules: true
```

## 3 上传至仓库

等待网站生成。
