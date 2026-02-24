---
# PDF Start
# title: BookStack 的 Https 配置
# author: 张年强
# subject: BookStack
# keywords: [Https, BookStack]
# PDF End
# Obsidian/Jekyll Start
title: BookStack 的 Https 配置
author: 张年强
date: 2026-02-23
last_modified_at: 2026-02-23
categories: [BookStack]
tags: [Https, BookStack]
description: 使BookStack能够使用Https访问的配置方法
# Obsidian/Jekyll End
---

## 1. 环境要求

|  **BookStack**   |    25.12.7     |
| :--------------: | :------------: |
|     **PHP**      |     ≥ 8.2      |
|   **MariaDB**    |     ≥ 10.6     |
|     **Git**      |      ---       |
|   **Composer**   |    ≥ 2.2.0     |
|   **OpenSSL**    |    ≥ 3.0.13    |
|    **Apache**    |    ≥ 2.4.0     |
|    **Ubuntu**    |     24.04      |
| **阿里云服务器** |      ---       |
|   **实名认证**   | 个人/企业 通过 |
|  **安全组规则**  |  443端口 允许  |

## 2. 证书

在阿里云网站登录自己的账号及密码。搜索`数字证书管理服务`并进入该页面。

### 2.1 领取

证书管理 -> SSL证书管理 V2.0 -> 个人测试证书 -> 购买证书 -> 立即购买。

### 2.2 申请

证书管理 -> SSL证书管理 V2.0 -> 个人测试证书 -> 申请 -> 提交审核。

### 2.3 验证

如果自动验证没有通过，按照页面信息，在域名增加解析条目。

### 2.4 下载

证书管理 -> SSL证书管理 V2.0 -> 个人测试证书 -> 下载 -> 选择 Apache 下载。

## 3. 服务器

### 3.1 部署证书

文件管理 -> `etc/ssl/certs` -> 右键 -> 上传 -> 选择证书文件。

### 3.2 配置Apache

打开 Apache 虚拟主机文件：

```
sudo nano /etc/apache2/sites-available/bookstack.conf
```

按照以下进行修改：

```apache
<VirtualHost *:443>

    ServerAdmin admin@example.com
    DocumentRoot /var/www/BookStack/public
    ServerName example.com
    ServerAlias www.example.com

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/example.com_public.crt
    SSLCertificateKeyFile /etc/ssl/certs/example.com.key
    SSLCertificateChainFile /etc/ssl/certs/example.com_chain.crt

    <Directory /var/www/BookStack/public/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

启用 Apache 虚拟主机文件：

```shell
sudo a2ensite bookstack.conf
```

SSL模块：

```shell
sudo a2enmod ssl
```

重写模块：

```shell
sudo a2enmod rewrite
```

重启Apache2服务以应用配置更改：

```shell
sudo systemctl restart apache2
```

## 4. BookStack

将目录更改为`/var/www/BookStack` ：

```shell
cd /var/www/BookStack/
```

打开`.env`：

```shell
sudo nano .env
```

修改：

```shell
APP_URL=https://example.com
```

保存并关闭。

重置URL：

```shell
php artisan bookstack:update-url http://example.com https://example.com
```

## 5. 测试

访问：<https://example.com>

Microsoft Edge 地址栏出现`锁`标志时，表示HTTPS功能成功。
