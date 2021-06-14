---
title: html2canvas实现网页截图，长按保存
date: '2018-08-13 18:59'
tag: 'Javascript'
---

传送门： https://github.com/Jensonhui/html2canvas-to-img

## 核心代码 ##
```javascript
/*图片跨域及截图模糊处理*/
let shareContent = document.getElementId('your el id'), //需要截图的包裹的（原生的）DOM 对象
width = shareContent.clientWidth, //shareContent.offsetWidth; //获取dom 宽度
height = shareContent.clientHeight, //shareContent.offsetHeight; //获取dom 高度
canvas = document.createElement("canvas"), //创建一个canvas节点
scale = 2; //定义任意放大倍数 支持小数
canvas.width = width * scale; //定义canvas 宽度 * 缩放
canvas.height = height * scale; //定义canvas高度 *缩放
canvas.style.width = shareContent.clientWidth * scale + "px";
canvas.style.height = shareContent.clientHeight * scale + "px";
canvas.getContext("2d").scale(scale, scale); //获取context,设置scale
let opts = {
	scale: scale, // 添加的scale 参数
	canvas: canvas, // 自定义canvas
	logging: false, // 日志开关，便于查看html2canvas的内部执行流程
	width: width,  // dom 原始宽度
	height: height, // dom 原始宽度
	useCORS: true // 【重要】开启跨域配置
        allowTaint: true
};

// 调用官方提供的方法
html2canvas(shareContent, opts).then((canvas) = > {
	var img = new Image();
	img.setAttribute('crossOrigin', 'anonymous');  // 解决跨域问题
	img.src = canvas.toDataURL("image/jpeg");  // 转化为base64编码
	document.body.appendChild(img);
})

// 长按保存
$(".saveimg").on({
	touchstart: function(e) {
		// 长按事件触发  
		timeOutEvent = setTimeout(function() {
			timeOutEvent = 0;
                        // console.log('你长按了');
                        // do something here...
		}, 400);
		//e.preventDefault();    
	},
	touchmove: function() {
		clearTimeout(timeOutEvent);
		timeOutEvent = 0;
	},
	touchend: function() {
		clearTimeout(timeOutEvent);
		if (timeOutEvent != 0) {
                        // console.log('你点击了');
                        // do something here...
		}
		return false;
	}
})
```

----------

### 使用方法

第一步：引入插件JS文件 
```javascript
<script src="js/html2canvas.js"></script>
<script src="js/html2Image.js"></script>
```

第二步：使用插件( 操作方法见上面 ↑ ↑ )

----------

### 注意事项

1 ) . html2canvas报错跨域问题，解决方案：代码在服务器运行

2 ) . html2canvas模糊问题，解决方案：自定义放大倍数，详细见上面代码demo

3 ) . html2canvas无法截全，解决方案： 放大widht，height，方法自行百度不唯一

4 ) . html2canvas可能无法截取到带有position，display:none, opacity:0等属性元素

更多解决方案： https://www.cnblogs.com/padding1015/p/9225517.html
