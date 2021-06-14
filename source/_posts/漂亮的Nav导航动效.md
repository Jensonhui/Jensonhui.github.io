---
title: 漂亮的Nav导航动效
date: '2017-06-10 18:02'
tag: ['Html', 'Css']
---

![请输入图片描述][1]

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Nav导航动效</title>
</head>
<style>
    *{margin:0;padding:0;}
    body,html{margin:0;padding:0;}
    ol li,ul li{float:left;margin:0;padding:0;width:25%;list-style:none;cursor:pointer;}
    ul{position:relative;z-index:10;margin:40px auto;width:400px;height:40px;border:1px solid #0067D0;border-radius:30px;text-align:center;line-height:40px;}
    ul li.on{color:#fff;transition:all .3s;}
    ul .back{position:absolute;top:0;left:0;z-index:-1;width:25%;height:40px;border-radius:30px;background-color:#0067D0;transition:all .3s;}
</style>
<body>
    <ul>
        <li class="on">首页</li>
        <li>关于</li>
        <li>介绍</li>
        <li>联系</li>
        <div class="back"></div>
    </ul>
</body>
<script>
    var lis = document.querySelectorAll("li");
    var backs = document.getElementsByClassName("back")[0];
    for (var i = 0; i<lis.length; i++){
        lis[i].i = i;
        lis[i].index = i;
        lis[i].onclick = function(){
            for(var j = 0; j<lis.length; j++){
                lis[j].classList.remove("on");
            }
            backs.style.left = (25 * this.index)+ "%";
            this.className = "on";
        }
    }
</script>
</html>

```


  [1]: http://blog.jensonhui.top/nav-beautiful.gif