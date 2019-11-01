---
title: Laravel项目部署（基于CentOS7）
date: 2019-10-28 20:45:45
tags:
- Laravel
- Linux
categories:
- Web开发
- 服务器运维
---
# 前言
在阿里云CentOS 7服务器上部署Laravel 5.8项目。
<!-- more -->

# 步骤
## 安装LAMP环境
建议参考此贴[centos7下利用yum搭建LAMP环境](https://segmentfault.com/a/1190000018781927)
## 安装Composer
+ 使用命令下载
```shell
curl -sS https://getcomposer.org/installer | php
```
+ 下载之后设置环境变量
```shell
mv composer.phar /usr/local/bin/composer
```
+ 修改权限，否则执行会出错
```shell
chmod -R 777 /usr/local/bin/composer
```
## 部署项目
这边使用git部署项目至Apache项目目录，在clone完毕后进入项目目录：
+ composer安装
```shell
composer install
```
若出现`Your requirements could not be resolved to an installable set of packages`，则运行`composer install --ignore-platform-reqs`或`composer update --ignore-platform-reqs`来忽略版本
+ 生成.env文件
```shell
cp .env.example .env
```
+ 生成key
```shell
php artisan key:generate
```
+ 开发环境改成生产环境
`APP_ENV=local`改成`APP_ENV=production`
+ 关闭调试模式
`APP_DEBUG=true`改成`APP_DEBUG=false`
+ 缓存配置文件
```php
php artisan config:cache // 配置缓存，生成：bootstrap/cache/config.php
php artisan config:clear // 清除配置缓存
```
+ 缓存路由文件
```php
php artisan route:cache // 路由缓存，生成：bootstrap/cache/routes.php
php artisan config:clear // 清除路由缓存
```
+ 性能优化
```php
php artisan optimize // 优化，生成编译文件；
```
+ 优化自动加载
```shell
composer dump-autoload --optimize
```
## 配置虚拟主机
```shell
<VirtualHost *:80>
        DocumentRoot  /var/www/html/your_project/public
        ServerName sample.com
        CustomLog logs/access_log combined
        DirectoryIndex  index.php index.html index.htm index.shtml
        <Directory "/var/www/html/your_project/public">
            Options FollowSymLinks
            AllowOverride All
        </Directory>
</VirtualHost>
```
## 分配权限
给所有项目分配777权限，具体权限说明请自行Google
```shell
sudo chmod -R 777 /var/www/html -a 
```