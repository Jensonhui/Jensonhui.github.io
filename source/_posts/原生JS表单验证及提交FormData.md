---
title: 原生JS表单验证及提交（ FormData ）
date: '2017-09-12 17:18'
tag: 'Javascript'
---

FormData的详细介绍及使用请[点击此处][1]，那里对FormData的方法和事件已经表述的非常清楚，

FormData对象可以让我们组织一个使用XMLHttpRequest对象发送的键值对的集合。它主要用于发送表单数据,但是可以独立于使用表单传输的数据。

**本文主要记录FormData对象的使用以及异步提交说明。**

----------

### 一、创建一个FormData对象

你可以创建一个你自己的FormData对象，然后通过append() 方法向对象中添加键值对，就像下面这样：

```javascript
    var formData = new FormData();

    formData.append("username", "Groucho");
    formData.append("accountnum", 123456); 

    formData.append("webmasterfile", blob);

    var request = new XMLHttpRequest();
    request.open("POST", "http://localhost/index.php");
    request.send(formData);

```

或者可以直接利用FormData对象直接提交一维数组到服务端，例如：


```javascript
// 实例化FormData对象，并将表单对象作为参数传入
var fd = new FormData(fm);

// 发送ajax请求，同时将表单的全部数据发送给后端
var xhr = new XMLHttpRequest();

//使用了formdata就必须使用post发送请求
xhr.open('post', 'http://localhost/index.php');

//调用send方法发送请求时，必须将fd作为参数传入
xhr.send(fd);

```

###  二、实战演练Demo

```Html
<form id="main" method="POST">
  <div>用户名: <input type="text" name="name" autocomplete="off"> <span></span></div>
  <div>密　码: <input type="password" name="pwd" autocomplete="off"> <span></span></div>
  <div>邮　箱: <input type="email" name="email" autocomplete="off"><span></span></div>
  <div>年　龄: <input type="text" name="age" autocomplete="off"> <span></span></div>
  <div><input id="btn" type="submit" value="注册"></div>
</form>



<script type="text/javascript">
    //判断浏览器中是否有 XMLHttpRequest 对象
    function getXhr() {
        var xmlhttp;
        if (window.XMLHttpRequest) {
            //如果有，说明是非IE浏览器 （chrome、Firefox、opera等，包括IE 7+）
            xmlhttp = new XMLHttpRequest();
        } else {
            //如果没有，说明是IE 7 -
            xmlhttp = new ActiveXObject('Msxml2.XMLHTTP');
        }
        return xmlhttp;
    }

    // 表单验证--姓名
    function checkName() {
        var tempName = this.value.trim(),
            errorTip = document.getElementsByName('name')[0].nextElementSibling,
            nameReg = /^[\u4E00-\u9FA5\uf900-\ufa2d·s]{2,20}$/;
        if (tempName.length > 2 && nameReg.test(tempName)) {
            errorTip.innerHTML = "";
            return;
        }
        errorTip.innerHTML = "请输入合法姓名";
    }

    // 表单验证--密码
    function checkPwd() {
        var tempName = this.value.trim(),
            errorTip = document.getElementsByName('pwd')[0].nextElementSibling;
        if (tempName.length > 6) {
            errorTip.innerHTML = "";
            return;
        }
        errorTip.innerHTML = "密码长度不能低于6位数";
    }

    // 表单提交--邮箱
    function checkEmail() {
        var tempName = this.value.trim(),
            errorTip = document.getElementsByName('email')[0].nextElementSibling,
            emailReg = /^[a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
        if (emailReg.test(tempName)) {
            errorTip.innerHTML = "";
            return;
        }
        errorTip.innerHTML = "请输入正确的邮箱地址";
    }

    // 表单提交--年龄
    function checkAge() {
        var tempName = this.value.trim(),
            errorTip = document.getElementsByName('age')[0].nextElementSibling,
            ageReg = /^[0-9]{1,2}$/;
        if (ageReg.test(tempName)) {
            errorTip.innerHTML = "";
            return;
        }
        errorTip.innerHTML = "请输入合法年龄";
    }

    window.onload = function () {
        document.getElementsByName('name')[0].onblur = checkName;
        document.getElementsByName('name')[0].onkeypress = checkName;

        document.getElementsByName('pwd')[0].onblur = checkPwd;
        document.getElementsByName('pwd')[0].onkeypress = checkPwd;

        document.getElementsByName('email')[0].onblur = checkEmail;
        document.getElementsByName('email')[0].onkeypress = checkEmail;

        document.getElementsByName('age')[0].onblur = checkAge;
        document.getElementsByName('age')[0].onkeypress = checkAge;
    }


    // 提交表单
    var SubmitBth = document.getElementById('btn');
    SubmitBth.onclick = function () {
        // 先获取表单的DOM对象
        var fm = document.getElementById('main'),
            inputs = fm.getElementsByTagName('input'),
            spans = fm.getElementsByTagName('span');

        // 验证表单是否为空 空值不允许提交
        for (var i = 0, len = inputs.length; i < len; i++) {
            inputs[i].index = i;
            if (inputs[i].value == '' || inputs[i].value == null) {
                alert('内容不能为空');
                return false;
            }
        }

        for (var j = 0, len = spans.length; j < len; j++) {
            if (spans[j].innerHTML != "") {
                alert("提交失败 ：" + spans[j].innerHTML);
                return false;
            }
        }

        // 实例化FormData对象，并将表单对象作为参数传入
        var fd = new FormData(fm);

        // 发送ajax请求，同时将表单的全部数据发送给后端
        var xhr = getXhr();

        //使用了formdata就必须使用post发送请求
        xhr.open('post', 'http://localhost/index.php');

        //调用send方法发送请求时，必须将fd作为参数传入
        xhr.send(fd);

        // 监听是否提交成功及返回值
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4 && xhr.status == 200) {
                console.log(xhr.responseText);
                if (xhr.responseText == 1) {
                    alert('提交成功');
                } else {
                    alert('提交失败')
                }
            }
        }
    }
</script> 
```



  [1]: https://developer.mozilla.org/zh-CN/docs/Web/API/FormData