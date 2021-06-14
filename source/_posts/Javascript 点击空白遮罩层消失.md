---
title: 点击空白区域遮罩层消失
date: '2017-09-28 17:56'
tag: 'Javascript'
---

**实现原理:** 判断点击的区域是否为需要隐藏内容区域 若不是则隐藏收起
```html
<div class="select-wrap">
	<div class="select-span">
		<span>选择一个</span>
	</div>
	<ul class="select-list" style="display:none;">
		<li>1111111111</li>
		<li>2222222222</li>
		<li>3333333333</li>
	</ul>
</div>

```

### JQurey方法
```javascript
<script src="http://cdn.static.runoob.com/libs/jquery/2.1.1/jquery.min.js"></script>
<script>
	var btn = $(".select-span");
	btn.on("click",function(){
		//这里可以添加判断 style.display == "block" ? "none" : "block";
		$(".select-list").stop().slideDown();
	})
	
  // 点击空白区域收起下拉ul  另外 移动端可能会实效 原因是不支持document 解决方案自行百度吧
	$(document).on('click', function(e){
		var contentEle= $('.select-wrap');
		if(contentEle!== e.target && contentEle.has(e.target).length === 0) {
			$('.select-wrap ul.select-list').css({'display':'none'});
		}
	});
</script>
```

<br/>

### Javascript方法
```html
<button id="btn">点我有特效</button>
<div id="cover"></div>
```

```javascript
<script>
  window.onload = function(){
      var Mybtn = document.getElementById('btn');
      var Mycover = document.getElementById('cover');
      Mybtn.onclick = function(){
        Mycover.style.display = "block";
        function moreddhide(e) {
            //如果e未定义，说明当前是IE浏览器 ,设置 e=window.event,其它浏览器中e就是event，所以不做处理
            if (!e) var e = window.event;      
            if (e.srcElement) {
                //ie support e.srcElement , e.sreElement指向触发事件的元素
                var a = e.srcElement.getAttribute("id")    
            } else {
                var a = e.target.getAttribute("id")    //ff support e.target
                alert(a);
            }
            if (a != 'btn') {
                Mycover.style.display = "none";
            }
        }
        document.onclick = moreddhide;
      }
  }
</script>

```