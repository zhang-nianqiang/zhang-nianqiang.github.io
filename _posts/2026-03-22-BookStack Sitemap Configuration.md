---
# PDF Start
# title: BookStack 的 Sitemap 配置
# author: 张年强
# subject: BookStack
# keywords: [BookStack, Sitemap]
# PDF End
# Obsidian/Jekyll Start
title: BookStack 的 Sitemap 配置
author: 张年强
date: 2026-03-22
last_modified_at: 2026-03-22
categories: [BookStack]
tags: [BookStack, Sitemap]
description: 使BookStack能够使用Sitemap的配置方法
# Obsidian/Jekyll End
---

## 1. Sitemap

Sitemap 是“网站地图”，一个包含网站页面的列表，一般是 `.XML` 格式，旨在帮助搜索引擎更有效地抓取和索引网站内容。制作并提交 Sitemap 的主要目的是为了让搜索引擎快速找到并收录网站中的页面。

BookStack 由于作者对其定位问题并未提供原生 Sitemap 功能，其提供的使用 API 形式生成 Sitemap 的方式并不简便，所以下面使用第三方服务介绍配置 Sitemap 的方法。

## 2 PRO-Sitemaps

PRO-Sitemaps 是一个建立很早的提供生成 Sitemaps 文件的网站。

- 访问：PRO-Sitemaps 网站。
- 填写：网站地址及邮箱地址，同意条款。
- 点击：注册。
- 访问：你的邮箱，激活账户。
- 等待：Sitemap 生成结束。
- 下载：`sitemap.xml` 文件。

> [!NOTE]
>
> PRO-Sitemaps 免费账户支持500页面，当你的网站内的网页超出的时候请注意使用其他方法。例如：[免费 XML Sitemap 生成器 – 在线创建站点地图](https://landing-page.io/zh/tools/sitemap-generator)

## 3 服务器

把 `sitemap.xml` 文件上传至你的服务器 `BookStack/public` 文件夹内。

访问：<https://zhangnianqiang.top/sitemap.xml> 验证文件路径正确。

## 4 Bing Webmasters Tools

- 访问：Bing Webmasters Tools。
- 选择：你的网站域名
- 选择：Sitemap
- 点击：提交 Sitemap
- 输入：<https://zhangnianqiang.top/sitemap.xml>
- 点击：提交。

等待一会之后会显示索引的结果。
