---
# PDF Start
# title: 使用GitHub Actions在GitHub Pages部署Docusaurus V3
# author: 张年强 
# subject: Internet Technology
# keywords: [Github Actions, Docusaurus v3, GitHub Pages]
# PDF End
# Github pages Jekyll Start
title: 使用GitHub Actions在GitHub Pages部署Docusaurus V3
author: 张年强
date: 2023-12-17 10:00:00 +0800
last_modified_at: 2024-01-29 10:00:00 +0800
categories: [Internet Technology]
tags: [Github Actions, Docusaurus v3, GitHub Pages]
image: 
  path: /githubpage/644.101-docusaurus-social-card.jpg
  lqip: /githubpage/644.101-docusaurus-social-card.jpg
# Github pages Jekyll End
---

## 前言

Docusuarus由于同时具有Blog和Doc两种形式,且支持多版本多语言,可以适配很多场景,所以使用的机构和个人越来越多.

[官方部署方法](https://docusaurus.io/docs/deployment)适合专业人士,作为非专业人士在部署上有一些专业需求:

- 能够自动生成和部署.
- 空间免费.
- 只使用简单的操作.

[Lars仓库](https://github.com/LayZeeDK/github-pages-docusaurus)中的方法很合适:

使用GitHub Actions自动将仓库内Docusuarus文件生成静态文件并部署到GitHub Pages,需要操作的部分只有将Docusuarus文件上传至GitHub仓库.

这个方法完美的满足了上面那些专业要求,感谢作者[Lars](https://github.com/LayZeeDK)!

下面就将介绍这个方法:

> [!IMPORTANT]  
>
> 与[Lars仓库](https://github.com/LayZeeDK/github-pages-docusaurus)中方法的主要区别:
>
> ① 由于Docusaurus V2和V3有些变化,做了更改.
>
> ② 由于GitHub Actions一些组件升级,做了更改.
>
> ③ 对于可能遇到的一些问题,增加解决方法.
>
> ④ 针对Windows系统环境.

---

## 第一步 生成Docusuarus网站：

### 方法一:网络生成

- 使用[示例仓库模板](https://github.com/Repair-Technology/docusaurus-v3/generate)新建仓库,使用GitHub Desktop克隆到本地.

  或

- 从[示例仓库](https://github.com/Repair-Technology/docusaurus-v3)下载(*Code->Download ZIP*)所有文件到本地,解压缩.

> [!IMPORTANT]  
>
> ① 使用方法一,可以跳过 `第三步 部署GitHub Actions` 步骤.
>
> ② 如果您想使用Docusuarus的最新版本,请使用 `方法二` .

### 方法二:本地生成

Windows本地推荐安装环境:

- [Node.js](https://nodejs.org/en) ≥20
- [yarn](https://yarn.nodejs.cn/en/docs/install#windows-stable)    ≥1.22.19 

使用以下命令:

```
yarn create docusaurus <folder-name> classic --typescript
```

> [!IMPORTANT]  
>
> ① 右键->以管理员身份运行Power Shell.
>
> ② `<folder-name>` 不能是已经存在的文件夹.
>
> ③ 检查是否存在 `yarn.lock` 如果不存在,在 `<folder-name>` 文件夹下使用以下命令生成此文件:
>
> ```
> yarn build
> ```
>
> ④ node_modules 不用上传至GitHub仓库.

## 第二步 配置Docusaurus

打开 `docusaurus.config.ts` .

配置 `Url` 和 `baseUrl` .

```ts
const config: Config = {
  // (...)
  url: `https://<github-organization-name>.github.io`,
  baseUrl = '/<repository-name>/`;
  };
```

配置 `organizationName` 和 `projectName` .

```ts
const config: Config = {
  // (...)
  organizationName: '<github-organization-name>',
  projectName: '<repository-name>',
};
```

配置Docs和Blog的 `editUrl` .

```ts
const config: Config = {
  // (...)
  presets: [
    [
      "classic",
      {
        docs: {
          // (...)
          editUrl: `https://github.com/<github-organization-name>/<repository-name>/tree/main/`,
        },
        blog: {
          // (...)
          editUrl: `https://github.com/<github-organization-name>/<repository-name>/tree/main/`,
        },
      },
    ],
  ],
};
```

## 第三步 部署GitHub Actions

新建: `.github/workflows/<workflow-name>.yml` 文件并粘贴以下内容:

```yaml
# Simple workflow for deploying static content to GitHub Pages
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: [main]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

env:
  # Hosted GitHub runners have 7 GB of memory available, let's use 6 GB
  NODE_OPTIONS: --max-old-space-size=6144

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: yarn
      - name: Install dependencies
        run: yarn install --frozen-lockfile --non-interactive
      - name: Build
        run: yarn build
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          # Upload entire repository
          path: build
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v3
```

## 第四步 配置GitHub仓库

选择GitHub Actions为构建和部署源.

*GitHub.com -> Repository -> Settings -> Pages -> Build and deployment -> Source -> GitHub Actions* 

## 第五步 上传至GitHub仓库

使用GitHub Desktop上传至Repository,等待网站生成.

---

