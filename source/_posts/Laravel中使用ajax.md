---
title: Laravel中使用ajax
date: 2019-10-05 20:43:44
tags:
- Laravel
- ajax
categories:
- Web开发
---

# 前言
Laravel中使用ajax实现表单验证。  
>当我们对 AJAX 的请求中使用 validate 方法时，Laravel 并不会生成一个重定向响应，而是会生成一个包含所有验证错误信息的 JSON 响应。这个 JSON 响应会包含一个 >HTTP 状态码 422 被发送出去。
<!-- more -->
# 代码实现

## 路由

`web.php`

```php
Route::get('ajaxRequest', 'HomeController@ajaxRequest');        //get方法，返回视图
Route::post('ajaxRequest', 'HomeController@ajaxRequestPost');   //post方法，处理ajax请求
```

## 控制器

`HomeController.php`
```php

    ......

    public function ajaxRequest(){
        return view('ajaxRequest');     
    }

    public function ajaxRequestPost(Request $request)
    {
        $validatedData = $request->validate([   //验证未通过会返回422错误
            'name' => 'required',
            'password' => 'required',
        ]);
        //return response()->json(['success' => '欢迎'.$input['name']]);  //关键核心：返回json信息
    }

    ......
```

## 视图
```html
    <form>
        <div class="form-group">
            <label>Name:</label>
            <input type="text" name="name" class="form-control" placeholder="Name" required="">
        </div>
        <div class="form-group">
            <label>Password:</label>
            <input type="password" name="password" class="form-control" placeholder="Password" required="">
        </div>
        <div class="form-group">
            <strong>Email:</strong>
            <input type="email" name="email" class="form-control" placeholder="Email" required="">
        </div>
        <div class="form-group">
            <button class="btn btn-success btn-submit">Submit</button>
        </div>
        <div id="response"></div>   <!--用来显示错误提示的div-->
    </form>
    ......
    <script>
        $.ajaxSetup({
            headers: {
                'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')    //携带csrf_token，否则会返回错误500
            }
        });

        $(".btn-submit").click(function (e) {
            e.preventDefault();
            var name = $("input[name=name]").val();
            var password = $("input[name=password]").val();
            var email = $("input[name=email]").val();
            $.ajax({
                type: 'POST',
                url: '/ajaxRequest',
                data: {name: name, password: password, email: email},
                success: function (data) {
                    alert(data.success);
                },
                error: data => {
                    if (data.status === 422) {
                        var errors = $.parseJSON(data.responseText);    //转json格式，或直接使用 data.responseJSON
                        $.each(errors, function (key, value) {
                            $('#response').addClass("alert alert-danger");
                            if ($.isPlainObject(value)) {
                                $.each(value, function (key, value) {
                                    console.log(key + " " + value);
                                    $('#response').show().append(value + "<br/>");

                                });
                            } else {
                                $('#response').show().append(value + "<br/>"); //this is my div with messages
                            }
                        });
                    }
                }
            });
        });
    </script>
```