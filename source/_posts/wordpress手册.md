---
title: wordpress手册
date: 2019-09-29 08:37:44
tags:
- wordpress
categories:
- Web开发
---
# 前言
本文是我个人整理的wordpress操作手册，用来记录常用的功能函数，不定期更新
<!-- more -->
# Wordpress常用特色功能

[官方网址](https://codex.wordpress.org/zh-cn:主题特性)

## 一、缩略图
### 1.开启缩略图
functions.php
```php
/**
* 第一个参数是缩略图，第二个是指定哪些信息类型开启缩略图功能  
*/
add_theme_support("post-thumbnails",array('post'));
```
### 2.获取缩略图
index.php
```php
<?php if(have_posts()):while(have_posts()):the_post(); ?>
    <div>
        <?php has_post_thumbnails() ?>  //可以传一个文章id作为参数
        <?php the_post_thumbnails() ?>
    </div>
```
`the_post_thumbnails($size)`  
#### 参数说明
$size：缩略图的尺寸，可用关键词或数值  
关键词：thumbnail，medium，large，full  
数值：array(x,y)

## 二、导航菜单
### 1.开启导航菜单
functions.php
```php
register_nav_menu($location,$description);
```
$location：导航位置
$description：描述，解释导航的信息

### 2.调用导航菜单
`wp_nav_menu($args)`  
$args : 
```php
array(
    'theme_location'=>'',   //根据参数选择对应location的导航
    'container'=>'nav' or 'div' or false,        //包围导航的元素
    'container_class'='',   //设置包围导航的元素的class
    'menu_class'=>'',       //设置ul的类名
    'echo'=>true,           //决定是否显示
    'before'=>'<h1>test</h1>',           //设置显示在a标签的内容
    'link_before'=>         //设置before内容的link
    'dephs'=>1,             //导航显示几个层级
)
```

## 三、侧边栏

### 1.开启侧边栏
```php
register_sidebar($args)
```
$args：
```php
array(
    'name'=>'',
    'id'=>'sidebar-1',
    'description'=>'',
    'class'=>'',
    'before_widget'=>'',//在侧边栏元素之前添加的内容
    'after_widget'=>'',
    'before_title'=>'',
    'after_title'=>''
)
```
### 2.调用侧边栏内容
```php
    dynamic_sidebar($index)   //调用并显示保存在指定侧边栏上的小工具
```

`$index` 可以是侧边栏的`name`或`id`
