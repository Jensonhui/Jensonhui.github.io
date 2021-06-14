---
title: Javascript 实现li呈圆形平均分布
date: '2018-04-28 15:26'
tag: ['Javascript', 'Css']
---

![请输入图片描述][1]
```html
<ul class="container">
    <li class="dot"></li> //dot中心远点
    <li class="box"></li> 
    <li class="box"></li>
    <li class="box"></li>
    <li class="box"></li>
    <li class="box"></li>
    <li class="box"></li>
</ul>
```

```css
html,body{
    margin: 0;
    padding: 0;
}
ul{
    width: 400px;
    height: 400px;
    padding: 0px;
    margin: 50px auto;
    position: relative;
    border-radius: 50%;
    border: 1px solid green;
}    
ul > li{
    position:absolute;
    width: 50px;
    height: 50px;
    list-style: none;
    border-radius: 50%;
    background-color: green;
}
ul > li.dot{
    background-color: #fff;
}

```

```javascript
<script src="https://cdn.bootcss.com/jquery/3.4.0/jquery.min.js"></script>
<script>
$(function () {
    //中心点横坐标
    var dotLeft = ($(".container").width() - $(".dot").width()) / 2;
    //中心点纵坐标
    var dotTop = ($(".container").height() - $(".dot").height()) / 2;
    //起始角度
    var stard = 0;
    //半径
    var radius = 200;
    //每一个BOX对应的角度;
    var avd = 360 / $(".box").length;
    //每一个BOX对应的弧度;
    var ahd = avd * Math.PI / 180;
    //设置圆的中心点的位置
    $(".dot").css({ "left": dotLeft, "top": dotTop });
    $(".box").each(function (index, element) {
        $(this).css({ 
            "left": Math.sin((ahd * index)) * radius + dotLeft, 
            "top": Math.cos((ahd * index)) * radius + dotTop 
        });
    });
})
</script>
```


  [1]: http://blog.jensonhui.top/js-circle.png