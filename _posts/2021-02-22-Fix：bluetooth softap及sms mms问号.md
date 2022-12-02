---
# PDF Start
# title: Fix：bluetooth softap及sms mms问号
# author: 张年强
# subject: Household Facilities
# keywords: [bluetooth-softap, sms-mms]
# PDF End
# Github pages Jekyll Start
title: Fix：bluetooth softap及sms mms问号
date: 2021-02-22 10:00:00 +0800
last_modified_at: 2022-11-08 10:00:00 +0800
categories: [Household Facilities]
tags: [bluetooth-softap, sms-mms] 
# Github pages Jekyll End
---

## 问题定义：

1. 设备管理器-其他设备-显示bluetooth softap及sms mms问号
2. 测试OS：Windows 10

## 处理过程：

解决方案一：删除已经配对的蓝牙设备

1. 控制面板-设备和打印机
2. 删除已经配对的蓝牙设备
3. 删除后设备管理器-其他设备-bluetooth softap及sms mms消失

解决方案二：取消服务

1. 控制面板-设备和打印机
2. 蓝牙设备-属性-服务
3. SMS/MMS及bluetooth softap前面的勾去掉

解决方案三：更新驱动 未测试

1. 控制面板-管理工具-服务
2. Bluetooth Support Service，启动并修改为自动
3. 更新蓝牙驱动

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
| 检查等级 | B                                                 |
| 更新等级 | B                                                 |
| 生命周期 | 更新                                              |
| 文档编号 | 99112-00006                                       |
| 文档版本 | 1.2.0.20221108                                    |
| 版权声明 | 作者保留所有权利                                  |
| 免责声明 | 作者不对任何不良后果负责                          |
| 更新日志 | 1.2.0.20221108 增加YAML/修改内容/修改文档信息     |
|          | 1.1.0.20220928 更换开源字体/修改内容/修改文档信息 |
|          | 1.0.0.20210222 首次发布                           |