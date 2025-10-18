---
# PDF Start
# title: 在阿里云 Ubuntu 24.04 服务器部署 WordPress
# author: 张年强
# subject: WordPress
# keywords: [阿里云, Ubuntu_24_04, WordPress]
# PDF End
# Obsidian/Jekyll Start
title: 在阿里云 Ubuntu 24.04 服务器部署 WordPress
author: 张年强
date: 2025-10-19
last_modified_at: 2025-10-19
categories: [WordPress]
tags: [阿里云, Ubuntu_24_04, WordPress]
# Obsidian/Jekyll End
---

## 1 运行要求

- **WordPress** 6.8.3
- **PHP** >= 8.3
- **MariaDB** >= 10.6

---

## 2 服务器

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

#### 2.2.4 更新操作系统

```shell
sudo apt-get update -y
```

```shell
sudo apt-get upgrade -y
```

安装完成后，在阿里云服务器控制面板重启服务器。

---

## 3 环境

安装 Apache Web 服务器、MariaDB 服务器、PHP 和其他 PHP 模块、Git。

```shell
sudo apt-get install apache2 mariadb-server php8.3 libapache2-mod-php8.3 php8.3-common php8.3-sqlite3 php8.3-curl php8.3-intl php8.3-mbstring  php8.3-xmlrpc php8.3-mysql php8.3-gd php8.3-xml php8.3-cli php8.3-opcache php8.3-zip unzip wget git -y
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

> [!TIP]
>
> 编辑完成后按下 Ctrl+X 组合键，输入 Y 确认保存修改，然后按下 Enter 键退出文件。

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

为 Bookstack 创建一个数据库：wordpressdb。

```mariadb
MariaDB [(none)]> CREATE DATABASE wordpressdb;
```

为 Bookstack 创建一个用户：wordpress。

```mariadb
MariaDB [(none)]> CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'password';
```

> [!TIP]
>
> 用自己的密码替换上面命令中的 password。

授予用户 wordpress 在数据库 wordpressdb 的所有权限：

```mariadb
MariaDB [(none)]> GRANT ALL ON wordpressdb.* TO 'wordpress'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

> [!TIP]
>
> 用您在上面使用的相同密码再次替换此处的 password。

刷新权限 MariaDB shell：

```mariadb
MariaDB [(none)]> FLUSH PRIVILEGES;
```

退出 MariaDB shell：

```mariadb
MariaDB [(none)]> EXIT;
```

## 4 部署 WordPress

### 4.1 下载 WordPress

浏览器打开-> [下载 – WordPress.org China 简体中文](https://cn.wordpress.org/download/)

选择“下载 WordPress 6.8.3”

### 4.2 上传 WordPress

选择“打开新文件管理”->定位到“/var/www”->“上传文件”->选择下载的“wordpress-6.8.3-zh_CN.zip”

### 4.3 解压缩 WordPress

将目录更改为/var/www/ ：

```shell
cd /var/www/
```

解压缩：

```shell
unzip wordpress-6.8.3-zh_CN.zip
```

### 4.4 目录权限

为 wordpress 目录授予适当的权限：

```shell
sudo chown -R www-data:www-data /var/www/wordpress/
```

```shell
sudo chmod -R 755 /var/www/wordpress/
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

#### 4.5.2 为 wordpress 配置 Apache

创建一个 Apache 虚拟主机文件：

```shell
sudo nano /etc/apache2/sites-available/wordpress.conf
```

添加以下行：

```xml
<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/wordpress
     ServerName example.com
     ServerAlias www.example.com

    <Directory /var/www/wordpress/>
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
sudo a2ensite wordpress.conf
```

重写模块：

```shell
sudo a2enmod rewrite
```

重新启动 Apache Web 服务以应用所有更改：

```shell
sudo systemctl restart apache2
```

### 4.6 安装 WordPress

在浏览器中输入：<http://example.com>

选择“现在就开始”->输入“数据库名称”“数据库用户名”“数据库密码”

点击“提交”->点击“运行安装程序”

填写信息->点击“安装 WordPress”

---

## 5 测试

访问：<http://example.com>

访问：<http://example.com/wp-login.php>

填写“账号及密码” ->点击“登录”
