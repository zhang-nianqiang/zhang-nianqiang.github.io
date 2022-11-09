---
# PDF Start
# title: Fix：Dell笔记本电脑BIOS不支持升级Windows10
# author: 张年强
# subject: Household Facilities
# keywords: [Dell笔记本电脑,Windows10,BIOS]
# PDF End
# Github pages Jekyll Start
title: Fix：Dell笔记本电脑BIOS不支持升级Windows10
date: 2020-08-25 10:00:00 +0800
last_modified_at: 2022-11-03 10:00:00 +0800
categories: [Household Facilities]
tags: [Dell笔记本电脑,Windows10,BIOS] 
# Github pages Jekyll End
---

## 问题定义：

1. 使用设置-更新和安全-Windows更新升级至Windows 10 v2004时提示：不成功
2. 使用微软Windows10易升升级至Windows 10 v2004时提示：BIOS不支持
3. 环境描述：
   - 笔记本电脑：Dell Inspiron 15R N5110
   - 操作系统：Windows 10 v1909
   - BIOS：A6

## 处理过程：升级BIOS

1. 下载BIOS最新版本A11
   - 下载链接:<https://dl.dell.com/FOLDER06292082M/1/N5110A11.EXE>
   > 20220913 ：由于未知原因，Dell官方网站恢复了A11版本的下载链接且修改了A11的更新日期
   - 下载链接:<https://pan.baidu.com/s/1ShMbfqL67NyZcSyGMHj9hA>提取码: AXSD
   > 20200825 ：由于未知原因，Dell官方网站移除了A3以上版本的下载链接且修改了A3的更新日期
2. 关闭正在运行的应用
3. 确保电源持续稳定
4. 运行BIOS程序进行安装
5. 等待
6. 安装结束后自动重启
7. 使用微软Windows 10易升升级
8. 无错误提示，正常安装
9. Windows 10升级结束

<div style="page-break-after: always;"></div>

## 文档信息：

| 文档作者 | 张年强                                            |
| -------: | :------------------------------------------------ |
| 文档分类 | 99000-Technology                                  |
|          | 99100-Household Facilities                        |
|          | 99110-Computer                                    |
|          | 99112-Software                                    |
| 存储等级 | B                                                 |
| 保密等级 | D                                                 |
| 检查等级 | X                                                 |
| 更新等级 | X                                                 |
| 生命周期 | 锁定                                              |
| 文档编号 | 99112-00001                                       |
| 文档版本 | 2.0.0.20221103                                    |
| 版权声明 | 作者保留所有权利                                  |
| 免责声明 | 作者不对任何不良后果负责                          |
| 更新日志 | 2.0.0.20221103 增加YAML/修改内容/修改文档信息     |
|          | 1.1.0.20220913 更换开源字体/修改内容/修改文档信息 |
|          | 1.0.0.20200825 首次发布                           |
