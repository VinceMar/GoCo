---
title: Ajax实现表格增删改查
date: 2019-09-14 10:38:02
tags:
- PHP
- Ajax
categories:
- Web开发
---

# 前言  

最近跟着慕课网的教程（ https://www.imooc.com/learn/754 ）实现了一个在线表格的CRUD（PHP+Ajax实现）。本文为总结帖，用来梳理思路以及记录细节。
<!-- more -->

# 正文

## 1.项目总览
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190914110321.png" width="70%"/>
<center>点击修改可以修改当前信息</center> 
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190914110442.png" width="70%"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190914110457.png" width="70%"/>

<center>点击右下角的按钮可以添加</center>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190914110608.png" width="70%"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190914110646.png" width="70%"/>

## 2.程序流程
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190914112851.png"/>
有关于Ajax提交数据和Form表单提交数据的区别在这里就不概述了，接下来具体要讲的就是使用jQuery的Ajax的细节该如何实现
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/Ajax%E7%BC%96%E8%BE%91%E8%A1%A8%E6%A0%BC.jpg"/>

## 3.代码实现
main.js

```javascript
/**
*   从后端取得数据并交由create_row方法生成表格的dom列
*/
$.get(init_data_url, (data) => {
        let row_items = $.parseJSON(data);  //$.parseJSON() 函数用于将符合标准格式的的JSON字符串转为与之对应的JavaScript对象。
        for (let i = 0; i < row_items.length; i++) {
            let data_dom = create_row(row_items[i]);
            g_table.append(data_dom);   //append() 方法在被选元素的结尾插入指定内容。
        }
    });
```


```javascript
/**
*   create_row方法，遍历形参的JavaScript对象，并将数据拼凑成单元格
*/
function create_row(data_item) {
    ...
    let row_obj = $('<tr></tr>');   //创建一个DOM列对象
    for (const key in data_item) {  //forin方法，key对应对象的元素名，data_item[key]对应对象的元素值
            if (key != 'id') {
                let col_td = $('<td></td>');
                col_td.html(data_item[key]);
                row_obj.append(col_td);
            }
        }
    ...

    delButton.attr('dataid', data_item['id']);  //给删除和编辑按钮添加这一列元素的id
    editButton.attr('dataid', data_item['id']);

    ...

}
```

```javascript
/**
*   delHandler方法，删除按钮的点击事件
*/
    function delHandler() {
        let data_id = $(this).attr('dataid');   //获取按钮对应列数据的id
        $.post("data.php?action=del_row", {     //向后端路由传输action参数指定处理方法
            data_id: data_id    //参数id
        }, (res) => {
            if (res == 'ok') {
                $(this).parent().parent().remove();     //后端删除成功，前端找到对应按钮的列并移除
            } else {
                alert('删除失败');
            }
        });
    }
```

```javascript
/**
*   editHandler，编辑按钮的点击事件。实现的思路：点击按钮，生成一个新行，将原始数据填充进新行，最后用新行替换旧行
*/

function editHandler() {
    let data_id = $(this).attr('dataid');   //当前按钮的id
    let meRow = $(this).parent().parent();  //当前行对象

    let editRow = $('<tr></tr>');           //新行dom对象

    ...

    let saveBtn = $("<button class='btn btn-primary'>保存</button>");
    let cancelBtn = $("<button class='btn btn-warning'>取消</button>");

    saveBtn.click(function () {
    let currentRow = $(this).parent().parent();
    let input_fields = currentRow.find('input');    //找到全部的input
    let post_fields = {};       //保存input键值的对象
    for (let i = 0; i < input_fields.length; i++) {
    let val = input_fields[i].getAttribute('name');
    post_fields[val] = input_fields[i].value;
    }
    post_fields['id'] = data_id;    //保存当前行id，用于SQL语句的WHERE
        $.post("data.php?action=edit_row", post_fields, res => {
            if (res == 'ok') {
                let newUpdateRow = create_row(post_fields);     //用当前行的键值对象创建一个新行
                currentRow.replaceWith(newUpdateRow);       //用新行替换当前行
            } else {
                alert("插入失败");
            }
        });
    })

    cancelBtn.click(function () {
        let currentRow = $(this).parent().parent();
        meRow.find('button:eq(0)').click(editHandler);  //绑定事件
        meRow.find('button:eq(1)').click(delHandler);
        currentRow.replaceWith(meRow);
    });

}
```

data.php
```php
// router
$action = $_GET['action'];
switch ($action) {
    case 'init_data_list':
        init_data_list();
        break;
    case 'add_row':
        add_row();
        break;
    case 'del_row':
        del_row();
        break;
    case 'edit_row':
        edit_row();
        break;
}
```

```php
/**
*   从数据库读取数据，转换为json格式后传给前端
*/
function init_data_list()
{
    $data = array();
    $sql = "SELECT * FROM `money` ORDER BY `id` ASC";
    $query = query_sql($sql);
    while ($row = $query->fetch_assoc()) {
        $data[] = $row;
    }
    echo json_encode($data, true);
}
```

```php
/**
*   从数据库连接函数
*/
function query_sql()
{
    $mysqli = new mysqli("127.0.0.1", "root", "", "room");
    $sqls = func_get_args();    //返回全部参数的值,类型是数组
    foreach ($sqls as $item) {
        $query = $mysqli->query($item);
    }
    $mysqli->close();
    return $query;
}
```

```php
function add_row()
{
    $sql = 'INSERT INTO money (info,cost,time) VALUES("' . $_POST['info'] . '",' . $_POST['cost'] . ',"' . $_POST['time'] . '")';
    if ($res = query_sql($sql, "SELECT LAST_INSERT_ID() as LD")) {  //查询最后一项的id，起名为LD
        $d = $res->fetch_assoc();
        echo $d['LD'];  
    }
}
```