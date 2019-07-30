---
title: laravel学习记录
date: 2019-07-04 17:37:12
tags: 
- laravel
categories: 
- Web开发
---
# 前言
&emsp;&emsp;本帖用于记录学习laravel时遇到的各种问题，不定期更新
<!-- more -->
# 网页访问过程
当用户在查看一个网页时，一个完整的访问过程如下：

1. 打开浏览器在地址栏输入 URL 并访问；
2. 路由将 URL 请求映射到指定控制器上；
3. 控制器收到请求，开始进行处理。如果视图需要动态数据进行渲染，则控制器会开始从模型中读取数据；
4. 数据读取完毕，将数据传送给视图进行渲染；
5. 视图渲染完成，在浏览器上呈现出完整页面；
<!-- more -->
# laravel路由
```php
Route::get('/', 'StaticPagesController@home');
```
参数一：URL  
参数二：处理该URL的控制器动作  
+ GET 常用于页面读取
+ POST 常用于数据提交
+ PATCH 常用于数据更新
+ DELETE 常用于数据删除  

指向 web 路由文件中定义的 POST、PUT 或 DELETE 路由的任何 HTML 表单都应该包含一个 CSRF 令牌字段，否则，这个请求将会被拒绝。  

## 重定向|视图路由
Route::redirect 方法，可以通过第三个参数自定义返回码  
Route::view 方法，第二个参数数组中的数据会被传递给视图
```PHP
Route::redirect('/here', '/there', 301);
----------
Route::view('/welcome', ['name' => 'Taylor']);
```

## 路由参数
路由的参数放在 `{ }` 中，参数名只能为字母且不能有 `-` 符号  
路由参数会按顺序依次被注入到路由回调或者控制器中，而不受回调或者控制器的参数名称的影响。  
参数后加 `?` 意为可选参数（需给出默认值）
```php
//路由参数
Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    ···
});
//可选参数
Route::get('user/{name?}', function ($name = null) {
    return $name;
});
```
# laravel控制器
替代路由中由闭包所实现的逻辑处理  
单行为控制器需要声明一个`__invoke`方法，此时路由中无需指明控制器的对应方法