---
title: Web奇技淫巧, 常用整理
date: '2017-07-23 21:56'
tag: ['Html', 'Css', 'Javascript']
---

### 文字超出隐藏变...
```css
/* 强制不换行 */
white-space: nowrap;
/* 超出变省略号 */
text-overflow: ellipsis;
/* 英文单词换行 */
word-wrap: break-word;
overflow: hidden;

/*  webkit, 规定超出几行显示'···' */
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
overflow: hidden;
```

<br/>

### CSS3动画效果@keyframes
```css
/* 01：鼠标悬浮背景变化 */
.load-more:hover{
    animation:change .5s ease-in;
}
@keyframes change{
    0%,20%{opacity:0.25;}
    30%,50%{opacity:0.55;}
    60%,80%{opacity:0.75;}
    90%,100%{opacity:1;}
}


/* 02：3D旋转 */
.earth-round{
    -webkit-animation:earthmove 2s linear both infinite;
}
@keyframes earthmove{
    to{transform:rotateY(360deg)translateZ(20px)};
}


/* 03：360度旋转 */
.earth-round{
    -webkit-animation:earthmove 2s linear both infinite;
}
@-webkit-keyframes earthmove{
    0% {-webkit-transform:rotate(0deg);}
    50% {-webkit-transform:rotate(180deg);}
    100% {-webkit-transform:rotate(360deg);}
}

```

<br/>

### 清楚浮动 Clearfix
```css
.clearfix:after{content:".";display:block;height:0;clear:both;visibility:hidden}
.clearfix{*+height:1%;}
```

<br/>

### 文字垂直居中

```css
/* 1.0 定位，固定宽高 */
position: absolute;
width: 100px;
height: 100px;
top: 50%;
left: 50%;
margin: 0 auto;
margin-top: -50px;
margin-left: -50px;


/* 2.0 定位，宽高不固定 */
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%)


/* 3.0 利用flex属性 */
display: flex;
justify-content: center;
align-items: center;

/* 4.0 利用display属性 */
<div class="box">
  <div class="content">
    <div class="inner"></div>
  </div>
</div>

.box{background-color:#ff8c00;width:300px;height:300px;display:table}
.content{background-color:red;display:table-cell;vertical-align:middle;text-align:center}
.inner{background-color:#000;display:inline-block;width:20%;height:20%}
```

<br/>

### 浏览器input placeholder颜色重置
```css
input::-webkit-input-placeholder{color:#fff}
input:-moz-placeholder{color:#fff}
input::-moz-placeholder{color:#fff}
input:-ms-input-placeholder{color:#fff}
```

<br/>

### 自定义input[type="checkbox"]

演示地址：http://js.itivy.com/jiaoben1503/index.html

```css
<input type="checkbox" id="checkbox-all"><label for="checkbox-all"></label>

.table input[type=checkbox]{display:none}
.table input[type=checkbox]~label{display:inline-block;width:18px;height:18px;color:#fff;margin-right:5px;cursor:pointer;line-height:18px;text-align:center;border-radius:5px;background-color:#ccc}
.table input[type=checkbox]:checked+label{background-color:#0067d0}
.table input[type=checkbox]+label:before{content:"\2714"}
```

<br/>

### 常用CSS预设
```css
a,abbr,acronym,address,applet,article,aside,audio,b,big,blockquote,body,button,canvas,caption,center,cite,code,dd,del,details,dfn,div,dl,dt,em,embed,fieldset,figcaption,figure,footer,form,h1,h2,h3,h4,h5,h6,header,hgroup,html,i,iframe,img,input,ins,kbd,label,legend,li,main,mark,menu,nav,object,ol,output,p,pre,q,ruby,s,samp,section,small,span,strike,strong,sub,summary,sup,table,tbody,textarea,tfoot,th,thead,time,tr,tt,u,ul,var,video {
	margin: 0;
	padding: 0;
	border: 0;
	vertical-align: baseline;
	box-sizing: border-box;
	font-style: normal;
	font-weight: 400;
	text-decoration: none;
	outline: 0;
	border-radius: 0;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	-webkit-appearance: none;
	-webkit-overflow-scrolling: touch
}

aside,details,figcaption,figure,footer,header,hgroup,main,menu,nav,section{
  display:block;
}

body{line-height:1}
ol,ul{list-style:none}
blockquote,q{quotes:none}
a:active,a:hover{outline:0}
table{border-collapse:collapse;border-spacing:0}
blockquote:after,blockquote:before,q:after,q:before{content:'';content:none}
```

<br/>

### input只允许输入正整数/规定数字大小
```javascript

onkeyup="this.value=this.value.replace(/\D/g,'')" 
onafterpaste="this.value=this.value.replace(/\D/g,'')"

// 规定允许输入大小值
$('.threety').on('keyup',function(){
    var v = parseInt($(this).val(), 10);
    if( v > 30){
        $(this).val(30);
    }
});
```

<br/>

### Javascript 快速求数组和
```javascript
eval(arry.join('+'));
```

<br/>

### 判断滚动事件(回到顶部 + 检测滚动底部)
```javascript
let scrollBody = document.getElementById('ID')
scrollBody.onscroll = () => {
  // 获取距离顶部的距离
  let scrollHeight = scrollBody.scrollTop
  // 获取可视区的高度
  let viewHeight = scrollBody.clientHeight
  // 获取滚动条的总高度
  let bodyHeight = scrollBody.scrollHeight
  // 超出固定高度显示回到顶部
  scrollHeight > 600 ? this.showFlag = true : this.showFlag = false
  // 判断滚动到页面底部执行方法
  if (scrollHeight + viewHeight >= bodyHeight) {
    // do something here
  }
}
```