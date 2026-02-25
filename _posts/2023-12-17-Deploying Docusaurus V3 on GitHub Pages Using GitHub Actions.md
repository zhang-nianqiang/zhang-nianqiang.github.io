---
# PDF Start
# title: 使用 GitHub Actions 在 GitHub Pages 部署 Docusaurus V3
# author: 张年强
# subject: Docusaurus
# keywords: [Github_Actions, Docusaurus_v3, GitHub_Pages]
# PDF End
# Obsidian/Jekyll Start
title: 使用 GitHub Actions 在 GitHub Pages 部署 Docusaurus V3
author: 张年强
date: 2023-12-17
last_modified_at: 2026-02-26
categories: [Docusaurus]
tags: [Github_Actions, Docusaurus_v3, GitHub_Pages]
description: 使用 GitHub Actions 在 GitHub Pages 部署 Docusaurus V3 的方法
# Obsidian/Jekyll End
---

> [!CAUTION]
>
> 注意：本文档在2026年2月26日标记为`存档`，使用时请注意文内信息的准确性及时效性。

## 前言

Docusuarus由于同时具有Blog和Doc两种形式,且支持多版本多语言,可以适配很多场景,所以使用的机构和个人越来越多。

为了方便部署，使用[Lars](https://github.com/LayZeeDK/github-pages-docusaurus)的方法很合适:

使用GitHub Actions自动将仓库内Docusuarus文件生成静态文件并部署到GitHub Pages,需要操作的部分只有将Docusuarus文件上传至GitHub仓库。

下面就将介绍这个方法:

> [!IMPORTANT]
>
> 与Lars中方法的主要区别：
>
> ① 由于Docusaurus V2和V3有些变化,做了更改。
>
> ② 由于GitHub Actions一些组件升级,做了更改。
>
> ③ 对于可能遇到的一些问题,增加解决方法。
>
> ④ 针对Windows系统环境。

## 1. 生成Docusuarus网站

Windows本地推荐安装环境：

- [Node.js](https://nodejs.org/en) ≥20
- [yarn](https://yarn.nodejs.cn/en/docs/install#windows-stable) ≥1.22.19

使用以下命令：

```shell
yarn create docusaurus <folder-name> classic --typescript
```

> [!IMPORTANT]
>
> ① 右键->以管理员身份运行Power Shell。
>
> ② `<folder-name>` 不能是已经存在的文件夹。
>
> ③ 检查是否存在 `yarn.lock` 如果不存在,在 `<folder-name>` 文件夹下使用以下命令生成此文件：
>
> ```shell
> yarn build
> ```
>
> ④ node_modules 不用上传至GitHub仓库。

## 2. 配置Docusaurus

打开`docusaurus.config.ts` 。

配置 `Url` 和 `baseUrl` 。

```ts
const config: Config = {
  // (...)
  url: `https://<github-organization-name>.github.io`,
  baseUrl = '/<repository-name>/`;
  };
```

配置 `organizationName` 和 `projectName` 。

```ts
const config: Config = {
  // (...)
  organizationName: "<github-organization-name>",
  projectName: "<repository-name>",
};
```

配置Docs和Blog的 `editUrl` 。

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

## 3. 部署GitHub Actions

新建: `.github/workflows/<workflow-name>.yml` 文件并粘贴以下内容：

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
          node-version: 20
          cache: yarn
      - name: Install dependencies
        run: yarn install --frozen-lockfile --non-interactive
      - name: Build
        run: yarn build
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload entire repository
          path: build
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## 4. 配置GitHub仓库

选择GitHub Actions为构建和部署源。

_GitHub.com -> Repository -> Settings -> Pages -> Build and deployment -> Source -> GitHub Actions_

## 5. 上传至GitHub仓库

使用GitHub Desktop上传至Repository，等待网站生成。
