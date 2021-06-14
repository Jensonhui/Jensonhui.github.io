---
title: Css实现梯形Tab标签
date: '2017-08-17 18:39'
tag: 'Css'
---

实现效果：
![css实现梯形tab标签—jensonhui's blog][1]

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div class="box">
        <ul>
            <li>aaaa</li>
            <li>bbbb</li>
            <li>cccc</li>
            <li>dddd</li>
        </ul>
    </div>
</body>

<style>
*{padding:0;margin:0}
ul{margin:100px;height:20px;list-style:none;padding-left:20px}
li{text-align:center;width:50px;height:0;float:left;margin-left:-25px;border-width:0 20px 37px 27px;border-color:transparent transparent red;border-style:none solid solid}
ul li:first-child{margin-left:0}
</style>

</html>
```


  [1]: http://blog.jensonhui.top/css-tab.png