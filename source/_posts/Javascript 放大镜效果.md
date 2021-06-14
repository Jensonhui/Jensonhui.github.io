---
title: 腾讯课堂—Javascript放大镜
date: '2018-01-03 22:46'
tag: ['Javascript', 'Css']
---

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>放大镜效果</title>
    </head>

    <style>
        *{margin:0;padding:0;}
        body{
            background:url('https://cn.bing.com/az/hprichbg/rb/WolongPanda_ZH-CN10957042976_1920x1080.jpg') center no-repeat;
            background-size:100%;overflow:hidden;
        }
        #demo{
            width:200px;height:200px;border-radius:50%;
            box-shadow:0px 0px 20px 5px #000 inset,0px 0px 10px 3000px #000;
            /*水平偏移 垂直偏移 阴影模糊值 阴影外延值 阴影的颜色 inset内阴影*/
            cursor:pointer;transition:transform .5s;
        }
        #demo:active{transform:scale(2);}
        h2{position:relative;color:#fff;text-align:center;letter-spacing:2px;margin-top:30px;}
    </style>

    <body>
        <h2>长按鼠标左键放大</h2>
        <div id="demo"></div>
    </body>
    
    <script type="text/javascript">
        // 1.创建一个盒子，作为放大镜
        // 2.为盒子添加box-shadow 属性，遮罩可视区域，造成阴影的假象
        // 3.鼠标移动是，获取坐标点位置
        // 4.盒子的移动位置属性(盒子距离上、左的间距) = 当前鼠标移动的坐标
        var box = document.getElementById('demo');
        window.addEventListener('mousemove',function(e){
            var x = e.clientX;
            var y = e.clientY;
            box.style.marginLeft = (x-100) + "px";
            box.style.marginTop = (y-100) + "px";
        })
    </script>
</html>
```