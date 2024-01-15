---
# PDF Start
# title: 使用Chirpy Theme在GitHub Pages部署Jekyll
# author: 张年强 
# subject: Workflow
# keywords: [Chirpy Theme, Jekyll, GitHub Pages]
# PDF End
# Github pages Jekyll Start
title: 使用Chirpy Theme在GitHub Pages部署Jekyll
author: 张年强
date: 2023-12-23 10:00:00 +0800
last_modified_at: 2024-01-13 10:00:00 +0800
categories: [Workflow]
tags: [Chirpy Theme, Jekyll, GitHub Pages]
image: 
  path: /githubpage/16511.01-00002-jekyll-og.png
  lqip: /githubpage/16511.01-00002-jekyll-og.png
# Github pages Jekyll End
---

## 前言

Jekyll由于被GitHub Pages在底层支持,具备多种简单的部署方式并且免费,可以适配很多场景,所以使用的机构和个人越来越多.

Gem Based;Remote Theme;Theme Template,我们一般就是这三种方法在GitHub Pages部署Jekyll.

对于主题作者及主题使用者来说使用Theme Template也就是'主题模板'这种方式是最简单的:主题作者容易定位问题;使用者无需安装本地环境,5分钟就可以把博客搭建成功,只等内容建设.

下面就介绍使用[Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)主题在GitHub Pages部署的过程.感谢[Cotes Chung](https://github.com/cotes2020)的工作.

---

## 新建仓库

打开[Chirpy Start](https://github.com/cotes2020/chirpy-starter/generate) -> 键入仓库名称.

> [!IMPORTANT]
>
> 仓库名使用: <用户名.github.io>.

## 配置GitHub仓库

选择GitHub Actions为构建和部署源.

*GitHub.com -> Repository -> Settings -> Pages -> Build and deployment -> Source -> GitHub Actions* 

## 设置Chirpy Theme

### 基础设置

打开 `_config.yml` :

```
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
> `url` 不能以 `/` 结尾.

### 使用图片CDN功能

当使用图库时可以使用图片CDN功能,可以减少图片链接的固定输入工作.

打开 `_config.yml` :

```
img_cdn: https://XXX.com
avatar: '/XXX.jpg'
```

> [!IMPORTANT]  
>
> 图片链接使用时以 `/` 开头.

### 增加评论功能

方法一:使用与博客相同的仓库增加评论功能

方法二:使用与博客不同的仓库增加评论功能

作为非专业人士,选择方法二.

这样选择的好处是:

- 博客仓库的变更(包括更换主题及崩溃后重新部署),不影响评论数据的安全.

    > [!IMPORTANT]  
    >
    > 博客仓库名及文章名不能变更.

- 博客主题升级时可以采用`主题模板`方法重新部署,不需要本地编译环境.

  > [!IMPORTANT]
  >
  > 升级前需要仓库整体备份,升级后修改变更部分.

新建仓库后按以下进行操作:

1. 选择使用Discussions.

   *GitHub.com -> Repository -> Settings -> General -> Features -> Discussions* 

2. 安装[giscus app](https://github.com/apps/giscus)到仓库.

3. 访问[giscus](https://giscus.app/zh-CN)设置.

4. 填写<用户名/仓库名>并选择参数.

5. 按照提示在 `_config.yml` 填写:

```
comments:
 active: 'giscus'
 giscus:
  repo: 用户名/仓库名
  repo_id: XXXXXXXX
  category: Announcements
  category_id: XXXXXXXX
  mapping: 'pathname'
  input_position: 'top'
  lang: zh-CN
  reactions_enabled: 1
```

> [!IMPORTANT]  
>
> ① 评论设置中的 `lang` 设置需要和本地化的 `lang` 一致.
>
> ② 所填内容与 `#` 需要有空格间隔.
>
> ③ 不同设置需要不同符号,单引号或双引号或无符号.

### 本地化

1. 打开 `_config.yml`:

```
lang: zh-CN
```

2. 在[jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)的 `_data/locales/` 复制 `zh-CN.yml` 文件到仓库相同文件夹上传.

### 删除社交图标

打开 `_data/contact.yml` ,在需要删除的社交图标前增加 `#`

```
# - type: twitter
#   icon: "fa-brands fa-x-twitter"
```

### 使用子模块

1. 打开 `_config.yml` :

```
assets:
  self_host:
    enabled: true
```

2. 打开 `.github/workflows/pages-deploy.yml` :

```
steps:
  - name: Checkout
    uses: actions/checkout@v4
    with:
      submodules: true
```

## 上传至仓库

等待网站生成.

---

