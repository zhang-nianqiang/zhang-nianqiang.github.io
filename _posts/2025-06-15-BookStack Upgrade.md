---
# PDF Start
# title: BookStack 升级
# author: 张年强
# subject: BookStack
# keywords: [升级, BookStack]
# PDF End
# Obsidian/Jekyll Start
title: BookStack 升级
author: 张年强
date: 2025-06-15
last_modified_at: 2025-11-10
categories: [BookStack]
tags: [升级, BookStack]
description: BookStack 的升级方法及升级过程中遇到问题的处理方法
# Obsidian/Jekyll End
---

## 1 常规升级

- 登录服务器

- 将目录更改为/var/www/BookStack：

  ```shell
  cd /var/www/BookStack
  ```

- 更新在安装中创建的存储库：

  ```shell
  git pull origin release
  ```

- 安装 PHP 依赖项：

  ```shell
  composer install --no-dev
  ```

- 更新数据库：

  ```shell
  php artisan migrate
  ```

- 清除系统缓存：

  ```shell
  php artisan cache:clear
  php artisan config:clear
  php artisan view:clear
  ```

## 2 问题处理

### 2.1 用户权限

报警内容：`fatal: detected dubious ownership in repository at '/var/www/BookStack'`

报警原因：项目所有者与现在的用户不一致。

解决方法：

```shell
git config --global --add safe.directory /var/www/BookStack
```

重新进行升级。

### 2.2 文件权限变化

报警内容：`error: The following untracked working tree files would be overwritten by merge:`

报警原因：文件权限不同。

解决方法：

```shell
git config core.fileMode false
```

```shell
git status
```

重新进行升级。
