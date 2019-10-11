---
title: Laravel模态框表单学习记录
date: 2019-10-11 09:07:49
tags:
- Laravel
categories:
- Web开发
---

# 前言
项目中实现了一个使用Bootstrap4模态框+Jquery日期时间插件+Ajax的表单，本文为笔记帖。

<!-- more -->
# 效果

<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011091528.gif"/>

# 实现
## 技术栈
Laravel+Mysql+Bootstrap4+jQuery

## 步骤及代码
### 第一步：模态框
```html
<button class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">新建作业</button>
    <!-- 模态框 -->
    <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">新建作业</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <form id="addWork">
                        <div id="response"></div>
                        <div class="form-group">
                            <label for="classid">课程名：</label>
                            <select class="form-control" name="classid">
                                <option value="ABCDEFG">移动开发</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="start">起始时间：</label>
                            <input type="text" name="start" class="form-control datetimepicker-input" id="datetimepicker-start" data-toggle="datetimepicker" data-target="#datetimepicker-start"/>
                        </div>
                        <div class="form-group">
                            <label for="end">截止时间：</label>
                            <input type="text" name="end" class="form-control datetimepicker-input" id="datetimepicker-end" data-toggle="datetimepicker" data-target="#datetimepicker-end"/>
                        </div>
                        <div class="form-inline">
                            <label for="num">第&nbsp;<input type="number" name="num" class="form-control" min="1" max="10"/>&nbsp;次作业</label>
                        </div>
                        <br>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-warning" data-dismiss="modal">关闭</button>
                            <button type="button" id="reset-button" class="btn btn-danger">重置</button>
                            <button type="button" id="submit-button" class="btn btn-primary">提交</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
```
#### 日期时间插件
其中，起始时间和截止时间使用的是这款jQuery插件：[Tempus Dominus](https://tempusdominus.github.io/bootstrap-4/),这款插件提供了Bootstrap风格的日期时间表单控件，使用这款插件首先要引入相关CDN：
```html
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.min.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/tempusdominus-bootstrap-4/5.0.1/js/tempusdominus-bootstrap-4.min.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/tempusdominus-bootstrap-4/5.0.1/css/tempusdominus-bootstrap-4.min.css" />
```

在表单中写入如下代码
```html
<div class="input-group date" id="datetimepicker1" data-target-input="nearest">
    <input type="text" class="form-control datetimepicker-input" data-target="#datetimepicker1"/>
    <div class="input-group-append" data-target="#datetimepicker1" data-toggle="datetimepicker">
        <div class="input-group-text"><i class="fa fa-calendar"></i></div>
    </div>
</div>
```
这段代码的重点是id的值，要和下面的JavaScript代码匹配上
```javascript
<script type="text/javascript">
    $(function () {
        $('#datetimepicker1').datetimepicker();
    });
</script>
```

如果无误，将有如下效果
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011092614.png"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011092701.png"/>

当然，这款插件有着众多配置项以应对不同需求，正如我需要将12小时制转变为24小时制，那么需要在`$('#datetimepicker1').datetimepicker();`中`datetimepicker()`的参数出传入option对象
```javascript
$('#datetimepicker-start').datetimepicker({ 
    sideBySide: true,
    format:"YYYY-M-D H:mm" 
});
```
关于format的时间格式，具体细节可参考网址：[Moment.js | Docs](https://momentjs.com/docs/#/displaying/format/)
其他配置项请查看官方网站：[Tempus Dominus](https://tempusdominus.github.io/bootstrap-4/)

#### 重置按钮
```html
<button type="button" id="reset-button" class="btn btn-danger">重置</button>
```

```javascript
    $('#reset-button').click(function () {
        $("#addWork input").val("");    //将表单的input的值清空
    })
```

### 第二步：Ajax响应

#### 前端提交Ajax请求以及处理Json响应数据

```javascript
    $("#submit-button").click(function (e) {
        e.preventDefault(); //阻止默认行为
        let classid = $("select[name=classid]").val();
        let start = $("input[name=start]").val();
        let end = $("input[name=end]").val();
        let num = $("input[name=num]").val();
        $.ajax({
            type: 'POST',
            url: '/addwork',    //控制器路由
            data: {
                classid: classid,
                start: start,
                end: end,
                num: num
            },
            success: data => {
                console.log(data);
                alert(data.msg);    //与控制器return redirect()->json(['msg'=>'消息']);相关
            }
        });
    });
```


Tips:如果想使用Laravel中的validate表单验证可以查看我以前的文章：`Laravel中使用ajax`

#### 数据处理及返回Json数据
这一部分的功能对应Laravel中编写的相关控制器代码
`WorkController.php`
```php
    public function store(Request $request){
        //新建作业表单验证
        if($request['start'] == ''||$request['end'] == ''){
            return response()->json(['msg'=>'起止时间不能为空！']);
        }
        if($request['num'] == ''){
            return response()->json(['msg'=>'作业次数不能为空！']);
        }
        //将日期时间字符串转换为时间戳后比较大小
        $start = strtotime($request['start']);
        $end = strtotime($request['end']);
        if($start > $end ){
            return response()->json(['msg'=>'截止时间不能早于起始时间！']);
        }
        Work::create($request->all());  //操作模型，在数据库中新建该条数据
        return response()->json(['msg'=>'作业新建成功！']);
    }
```
具体请看代码注释

