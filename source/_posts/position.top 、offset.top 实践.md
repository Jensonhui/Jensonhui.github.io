---
title: position.top 、offset.top 
date: '2017-09-30 11:12'
tag: ['Javascript', 'Css']
---

**实现原理：**

1 利用H5【data】属性, 插入列表图片 , 悬浮是动态获取赋值

2 主要用到了position.top方法 (相对于定位的父容器获取), 区别于offset.top

----------

**复习一下：** position 和 offset 区别 http://www.w3school.com.cn/jquery/css_offset.asp

position() 方法返回匹配元素相对于父元素的位置（偏移）、返回的对象包含两个整型属性：top 和 left，以像素计

offset() 方法返回或设置匹配元素相对于文档的偏移（位置）、返回第一个匹配元素的偏移坐标、top 和 left，以像素计

<br/>

```html
<table class="prize-list">
  <tbody>
    <tr class="gay" data-img="images/huace1.png">
      <td>1</td>
    </tr>
    <tr class="gay" data-img="images/huace2.png">
      <td>2</td>
    </tr>
  </tbody>
</table>

<!-- 定位右面悬浮展示窗展示图片 跟随鼠标切换 -->
<div class="imgshow">
  <img src="" alt="这里动态赋值" />
</div>
```

```css
.tablePrize{width:1130px;max-width:1130px;position:relative}
.tablePrize .imgshow{position:absolute;top:0;display:none;right:-210px;width:187px;z-index:2;border-radius:6px;background-color:#e0e0e0;transition:all .3s ease-in-out}
.tablePrize .imgshow img{width:187px;border:none}
.tablePrize .imgshow:before{position:absolute;content:'';border-width:10px 10px 10px 10px;border-style:solid;border-color:transparent transparent #f3f3f3 transparent;left:-20px;top:52px;transform:rotate(-90deg)}
```


```javascript
<script>
  $('.prize-list tbody tr').mousemove(function(){
    var dataval = $(this).data('img');
    var emelTop = $(this).position().top;
    $('.imgshow img').attr('src', dataval );
    $('.imgshow').stop().show().css({'top':emelTop});
  }).mouseout(function(){
    $('.imgshow').stop().hide();
  })
</script>
```
&nbsp;
![请输入图片描述][1]


  [1]: http://blog.jensonhui.top/tableimg.gif