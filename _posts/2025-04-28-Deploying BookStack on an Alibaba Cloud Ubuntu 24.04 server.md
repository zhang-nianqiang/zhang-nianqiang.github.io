---
# PDF Start
# title: 在阿里云 Ubuntu 24.04 服务器部署 BookStack
# author: 张年强
# subject: BookStack
# keywords: [阿里云, Ubuntu_24_04, BookStack]
# PDF End
# Obsidian/Jekyll Start
title: 在阿里云 Ubuntu 24.04 服务器部署 BookStack
author: 张年强
date: 2025-04-28
last_modified_at: 2026-01-23
categories: [BookStack]
tags: [阿里云, Ubuntu_24_04, BookStack]
description: 在阿里云 Ubuntu 24.04 服务器部署 BookStack 的方法
# 2025.11.29 更改代码块设置 MariaDB 至 Shell，Minimal Mistakes 不支持MariaDB
# Obsidian/Jekyll End
---

## 1. 运行要求

| **BookStack** | 25.02.2  |   25.12.1   |
| :-----------: | :------: | :---------: |
|    **PHP**    |  >= 8.2  |   >= 8.2    |
|  **MariaDB**  | >= 10.2  | ==>= 10.6== |
|   **MySQL**   |  >= 5.7  | ==>= 8.0==  |
|    **Git**    |   ----   |    ----     |
| **Composer**  | >= 2.2.0 |  >= 2.2.0   |

## 2. 服务器

### 2.1 购买服务器

默认你有一台阿里云的服务器。

### 2.2 服务器设置

#### 2.2.1 操作系统

选择 Ubuntu 24.04 64 位。

#### 2.2.2 访问规则

标准版：

云服务器管理控制台->网络与安全->安全组->管理规则->增加规则。

简洁版：

云服务器管理控制台->安全组->管理规则->增加规则。

增加 80 端口访问规则。

#### 2.2.3 登录服务器

远程连接：

通过 Workbench 远程连接。

> [!TIP]
>
> 首次需要重置密码。

#### 2.2.4 允许 GitHub 访问

打开 hosts 文件。

```shell
sudo nano /etc/hosts
```

增加：

```
140.82.112.3 github.com
```

保存并关闭文件。

> [!TIP]
>
> 编辑完成后按下 Ctrl+X 组合键，输入 Y 确认保存修改，然后按下 Enter 键退出文件。

#### 2.2.5 更新操作系统

```shell
sudo apt-get update -y
```

```shell
sudo apt-get upgrade -y
```

安装完成后，在阿里云服务器控制面板重启服务器。

## 3. 环境

安装 Apache Web 服务器、MariaDB 服务器、PHP 和其他 PHP 模块、Git。

```shell
sudo apt-get install apache2 mariadb-server php8.3 libapache2-mod-php8.3 php8.3-common php8.3-sqlite3 php8.3-curl php8.3-intl php8.3-mbstring php8.3-xmlrpc php8.3-mysql php8.3-gd php8.3-xml php8.3-cli php8.3-tidy php8.3-zip unzip wget git -y
```

### 3.1 配置 PHP

打开 php.ini 文件：

```shell
sudo nano /etc/php/8.3/apache2/php.ini
```

进行以下更改：

```ini
memory_limit = 512M
upload_max_filesize = 20M
max_execution_time = 360
date.timezone = Asia/Shanghai
```

保存并关闭文件。

### 3.2 配置 MariaDB

保护 MariaDB 的安全。

```shell
sudo mysql_secure_installation
```

回答以下问题：

```shell
Enter current password for root (enter for none): ENTER
Switch to unix_socket authentication [Y/n]: N
Change the root password? [Y/n]: N
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]: Y
Reload privilege tables now? [Y/n]: Y
```

登录到 MariaDB shell：

```shell
mysql -u root -p
```

提供您的根密码。

屏幕会转到以下提示符：

`MariaDB [(none)]>`

为 Bookstack 创建一个数据库：bookstackdb。

```shell
CREATE DATABASE bookstackdb;
```

为 Bookstack 创建一个用户：bookstack。

```shell
CREATE USER 'bookstack'@'localhost' IDENTIFIED BY 'password';
```

> [!TIP]
>
> 用自己的密码替换上面命令中的 password。

授予用户 bookstack 在数据库 bookstackdb 的所有权限：

```shell
GRANT ALL ON bookstackdb.* TO 'bookstack'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

> [!TIP]
>
> 用您在上面使用的相同密码再次替换此处的 password。

刷新权限 MariaDB shell：

```shell
FLUSH PRIVILEGES;
```

退出 MariaDB shell：

```shell
EXIT;
```

### 3.3 安装 Composer

```shell
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

## 4. 部署 BookStack

### 4.1 下载 BookStack

将目录更改为/var/www/ ：

```shell
cd /var/www/
```

从 GitHub 下载最新版本的 BookStack。

```shell
sudo git clone https://github.com/BookStackApp/BookStack.git --branch release --single-branch
```

### 4.2 安装依赖项

将目录更改为 BookStack：

```shell
cd BookStack
```

安装 PHP 所需的所有依赖项：

```shell
sudo composer install
```

### 4.3 配置环境文件

复制示例环境配置文件：

```shell
sudo cp .env.example .env
```

打开.env 文件：

```shell
sudo nano .env
```

更改网站域名：

```ini
APP_URL=http://example.com
```

数据库设置进行以下更改：

```ini
# Database details
DB_HOST=localhost
DB_DATABASE=bookstackdb
DB_USERNAME=bookstack
DB_PASSWORD=password
```

保存并关闭文件。

创建应用程序密钥：

```shell
sudo php artisan key:generate
```

输出：

```shell
**************************************
*     Application In Production!     *
****************shell**********************

 Are you sure you want to run this command?
 yes/no
```

选择 yes，最终输出：

```shell
Application key set successfully.
```

### 4.4 迁移数据库

```shell
sudo php artisan migrate
```

您应该看到以下输出：

```shell
**************************************
*     Application In Production!     *
**************************************

 Are you sure you want to run this command?
 yes/no
```

选择 yes，等待执行完毕。

为 BookStack 目录授予适当的权限：

```shell
sudo chown -R www-data:www-data /var/www/BookStack/
```

```shell
sudo chmod -R 755 /var/www/BookStack/
```

### 4.5 配置 Apache

#### 4.5.1 配置全局参数

打开 apache2.conf 文件。

```shell
sudo nano /etc/apache2/apache2.conf
```

增加：

```
ServerName 127.0.0.1
```

保存并关闭文件。

#### 4.5.2 为 Bookstack 配置 Apache

创建一个 Apache 虚拟主机文件：

```shell
sudo nano /etc/apache2/sites-available/bookstack.conf
```

添加以下行：

```xml
<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/BookStack/public
     ServerName example.com
     ServerAlias www.example.com

    <Directory /var/www/BookStack/public/>
        Options FollowSymlinks
        AllowOverride All
        Require all granted
    </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

> [!TIP]
>
> 将上面中的 <admin@example.com>，example.com 和 <www.example.com> 替换成你自己的。

保存并关闭文件。

启用 Apache 虚拟主机文件：

```shell
sudo a2ensite bookstack.conf
```

重写模块：

```shell
sudo a2enmod rewrite
```

重新启动 Apache Web 服务以应用所有更改：

```shell
sudo systemctl restart apache2
```

## 5. 测试

访问: <http://example.com>

账号: <admin@admin.com>

密码: password
