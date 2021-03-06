---
title: Laravel学习记录
date: 2019-07-04 17:37:12
tags: 
- Laravel
categories: 
- Web开发
---
# 前言
&emsp;&emsp;本帖用于记录学习laravel时遇到的各种问题，不定期更新
<!-- more -->
# laravel工作过程
当用户在查看一个网页时，一个完整的访问过程如下：

1. 打开浏览器在地址栏输入 URL 并访问；
2. 路由将 URL 请求映射到指定控制器上；
3. 控制器收到请求，开始进行处理。如果视图需要动态数据进行渲染，则控制器会开始从模型中读取数据；
4. 数据读取完毕，将数据传送给视图进行渲染；
5. 视图渲染完成，在浏览器上呈现出完整页面；


# 配置相关
## Vagrant + Homestead 下的 MySql 默认帐号和密码
用户名：homestead  
密码：secret  
命令行登陆：$ mysql -uhomestead -psecret
## 运行项目出现`500 Server Error`
检查是否存在`.env`文件,若不存在则新建一个

## 出现`No application encryption key has been specified.`
若文件根目录下没有 .env
1、.env.example 改名使用命令 copy 修改为 .env
2、使用命令 php artisan key:generate  获取密码，自动保存到 .env
3、将密码复制到config/app.php 中的key里面
4、重新运行，OK。
如有.env 的情况，直接进行第二步。
两种情况皆无，请重新安装laravel。

## Laravel-MIX 运行`npm run watch-poll`报错
执行以下命令：

1.$ rm -rf node_modules

2.$ yarn config set registry http://registry.cnpmjs.org

3.$ yarn install --no-bin-links
接下来打开 pakage.json 修改【去掉四处 cross-env 】

## 执行`composer dumpautoload`报错
错误信息：
```shell
PHP Fatal error:  Uncaught TypeError: Argument 1 passed to Composer\Autoload\ClassLoader::addClassMap() must be of the type array, integer given,......
```
解决：  
1. 进入项目 根目录：
2. rm -rf vendor/composer/autoload_*  
3. composer dumpautoload

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
## 资源控制器
补充资源控制器  
如果你想在默认的资源路由中增加额外的路由，你应该在 `Route::resource` 之前定义这些路由。否则由 resource 方法定义的路由可能会无意中优先于你补充的路由：
### store
store方法用于处理从表单传入的数据，其中可以做数据验证validate，确认密码的字段需要name为password_confirmation，这样就可以在validate的password的验证规则中填写confirm实现校验确认密码。  
#### 校验规则
 |规则|说明|示例|
 |----|----|----|
 |confirmed|验证的字段必须和 foo_confirmation 的字段值一致。例如，如果要验证的字段是 password，输入中必须存在匹配的 password_confirmation 字段。||
 |size|验证的字段必须具有与给定值匹配的大小。对于字符串来说，value 对应于字符数。对于数字来说，value 对应于给定的整数值。对于数组来说， size 对应的是数组的 count 值。对文件来说，size 对应的是文件大小（单位 kb ）。||
 |max|验证中的字段必须小于或等于 value。字符串、数字、数组或是文件大小的计算方式都用 size 方法进行评估。||
 |min|验证中的字段必须具有最小值。字符串、数字、数组或是文件大小的计算方式都用 size 方法进行评估。||
 |unique|unique:table,column,except,idColumn验证的字段在给定的数据库表中必须是唯一的。如果没有指定 column，将会使用字段本身的名称|'email' => 'unique:users,email_address'|
 |required|验证的字段必须存在于输入数据中，而不是空。如果满足以下条件之一，则字段被视为「空」： 该值为 null. 该值为空字符串。 该值为空数组或空的 可数 对象。 该值为没有路径的上传文件。||
 |email|验证的字段必须符合 e-mail 地址格式。||
 |sometimes|只有在该字段存在时， 才对字段执行验证||
 |nullable|如果你不希望验证程序将 null 值视为无效的，那就将「可选」的请求字段标记为 nullable，也可以理解为有值时才验证。||

若在某个属性第一次验证失败后停止运行验证规则，附加bail
```
$this->validate($request, [
    'title' => 'bail|required|unique:posts|max:255',
    'body' => 'required',
]);
```

 #### 验证错误提示消息

 ```php
@if (count($errors) > 0)
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
 ```
但是默认的提示消息是英文的，所以我们需要安装一个中文语言包
```
composer require caouecs/laravel-lang:~3.0
```
默认的错误提示消息为英文，所以使用时需要复制`vendor/caouecs/laravel-lang/src`下的语言包到`resource/lang`目录下，并修改`config/app.php`中`locale => 'zh-CN'`；当然也可以自己写验证消息，只需要在validate中传入第三个参数即可
```
$this->validate( $request, [
            'stunum'      => 'required|unique:users',
        ], [
            'stunum.required'      => '学号 不能为空',
        ] );
```

# 表单请求验证

如果校验规则比较复杂，可以运行`php artisan make:request StoreBlogPost`生成一个表单请求类，然后在控制器方法中类型提示传入的请求`public function store(StoreBlogPost $request)`  
如果验证失败，就会生成一个让用户返回到先前的位置的重定向响应。  
如果传入的请求是 AJAX，会向用户返回具有 422 状态代码和验证错误信息的 JSON 数据的 HTTP 响应。

重写messages方法可以自定义错误消息  
```
public function messages()
{
    return [
        'title.required' => 'A title is required',
        'body.required'  => 'A message is required',
    ];
}
```

# 加密
bcrypt每次获得的结果不同：
```php
$hash1 = bcrypt('secret')
$hash2 = bcrypt('secret')

Hash::check('secret', $hash1)
Hash::check('secret', $hash2)
```

# laravel模型关联

## 一对多
例如：
博客文章&评论是一对多关系：一篇文章可以有多个评论，但一个评论只能属于一篇文章  
```
class Post extends Model
{
    /**
     * 获得此博客文章的评论。
     */
    public function comments()
    {
        return $this->hasMany('App\Comment');
    }
}
```
```
class Comment extends Model
{
    /**
     * 获得此评论所属的文章。
     */
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
```

在上面的例子中，Eloquent 会尝试用 `Comment` 模型的 `post_id` 与 Post 模型的 `id` 进行匹配。默认外键名是 Eloquent 依据关联名、并在关联名后加上 `_id` 后缀确定的。当然，如果 Comment 模型的外键不是 `post_id`，那么可以将自定义键名作为第二个参数传递给 belongsTo 方法

# 控制器

资源控制器绑定模型：
```shell
php artisan make:controller PhotoController --resource --model=Photo
```

# 扩展包安装
1. 中文语言包
```
composer require "overtrue/laravel-lang:~3.0"
```
2. 验证码
```
composer require "mews/captcha:~2.0"
```  
运行以下命令生成配置文件 config/captcha.php：
```shell
php artisan vendor:publish --provider='Mews\Captcha\CaptchaServiceProvider'
```

3. font-awesome
```shell
yarn add --dev @fortawesome/fontawesome-free
```