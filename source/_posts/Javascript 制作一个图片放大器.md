---
title: JS制作一个图片放大器
date: '2020-02-27'
tag: 'Javascript'
---

```
<!DOCTYPE html>
<html>
<head>
  <title>图片放大器 --- jensonhui's blog</title>
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <div id="largePicture"></div>
</body>
<script type="text/javascript" src="largePictrue.js"></script>
<script type="text/javascript">
  // 调用
  let arry = [ 'img/0@2x.png', 'img/1@2x.png', 'img/2@2x.png', ]
  new setCreatDomFun({ imgListArry : arry })

  // 可配置的自定义选项
  imgListBox : 'imgBox', // 图片盒子ID
  showdowBox : 'bigBox', // 阴影盒子ID
  shadowLeftBtn : 'leftBtn', // 左按钮ID
  shadowRigtBtn : 'rightBtn', // 右按钮ID
  shadowImgBox : 'bigImgBox', // 阴影图片盒子ID
  shadowCloseBtn : 'closeBox' // 关闭阴影按钮ID
</script>
</html>
```

```
html, body {position: relative;}
/* 图片列表 */
#largePicture ul li{list-style:none;margin:0 5px;display:inline- block;width:150px;height:150px}
#largePicture ul li img{width:100%;height:100%}
/* 阴影弹框 */
#largePicture .shadow-box{position:fixed;top:0;left:0;width:100%;height:100%;z-index:100;display:none;background-color:rgba(0,0,0,.8)}
#largePicture .shadow-box .btn{position:absolute;top:50%;width:30px;height:50px;color:#fff;z-index:102;cursor:pointer;text-align:center;line-height:50px;border:1px solid #ccc;background-color:#000;transform:translate(-50%,-50%)}
#largePicture .shadow-box .leftBtn{left:12%;content:'>'}
#largePicture .shadow-box .rightBtn{right:12%;content:'<'}
#largePicture .shadow-box .bigImg{position:absolute;top:50%;left:50%;z-index:200;width:650px;height:580px;background-size:cover;background-repeat:no-repeat;transition:all .5s ease-in-out;transform:translate(-50%,-50%)}
#largePicture .shadow-box .closeBtn{position:absolute;top:20px;right:5%;z-index:240;color:#fff;cursor:pointer;padding:6px 25px;border:1px solid #fff}
/* 消息提示框 */
#toast-tip{position:absolute;top:10%;left:50%;font-size:14px;color:#fff;z-index:350;padding:10px 40px;transform:translateX(-50%);background-color:rgba(0,0,0,.8)}
```

```
(function(win, doc) {
	// 默认设置
	let defaultSettings = {
		imgListArry: [],
		imgListBox : 'imgBox',
		showdowBox : 'bigBox',
		shadowLeftBtn : 'leftBtn',
		shadowRigtBtn : 'rightBtn',
		shadowImgBox : 'bigImgBox',
		shadowCloseBtn : 'closeBox'
	}	

	let ops = {} // 合并option
	let chickImg = null
	let clickIndex = null

	function setCreatDomFun (options) {
		ops = Object.assign({}, defaultSettings, options)
		// 是否存在img数组
		if (ops.imgListArry.length <= 0) {
			this.showToast('您没有配置图片')
		}
		this.creatContainer()
	}

	setCreatDomFun.prototype = {
		// 创建DOM元素
		creatContainer : function () {
			let pictrueNode = `
				<ul id="${ops.imgListBox}"></ul><div id="${ops.showdowBox}" class="shadow-box">
					<div id="${ops.shadowLeftBtn}" class="btn leftBtn"></div>
					<div id="${ops.shadowRigtBtn}" class="btn rightBtn"></div>
					<div id="${ops.shadowImgBox}" class="bigImg"></div>
					<div id="${ops.shadowCloseBtn}" class="closeBtn"> 关闭 </div>
				</div>`
			let largePicDom = document.getElementById('largePicture')
			largePicDom.innerHTML = pictrueNode
			this.creatImgs()
		},
		// 遍历渲染img
		creatImgs : function () {
			let ulbox = document.getElementById(ops.imgListBox)
			for (let i = 0, len = ops.imgListArry.length; i < len; i++) {
				let lis = document.createElement('li')
				let imgs = document.createElement('img')
				imgs.src = ops.imgListArry[i]
				lis.appendChild(imgs)
				ulbox.appendChild(lis)
			}
			this.clickImgFun()
		},
		// 图片点击事件
		clickImgFun : function () {
			let shadowBox = document.getElementById(ops.showdowBox)
			let bigImgDom = document.getElementById(ops.shadowImgBox)
			let lis = document.getElementById(ops.imgListBox).children
			for (let j = 0, len = lis.length; j < len; j++) {
				lis[j].onclick = function () {
					clickIndex = j
					chickImg = lis[j].childNodes[0].src
					shadowBox.style.display = 'block'
					bigImgDom.style.backgroundImage = `url(${chickImg})`
				}
			}
			this.checkBigImg()
			this.closeShadowBox()
		},
		// 阴影切换事件
		checkBigImg: function () {
			let leftBtn = document.getElementById(ops.shadowLeftBtn)
			let rightBtn = document.getElementById(ops.shadowRigtBtn)
			let bigImgDom = document.getElementById(ops.shadowImgBox)
			let arryLen = ops.imgListArry.length

			if (!leftBtn || !rightBtn) { return false }

			leftBtn.onclick = function () {
				clickIndex --
				if (clickIndex <= 0) {
					clickIndex = arryLen - 1
				}
				bigImgDom.style.backgroundImage = `url(${ops.imgListArry[clickIndex]})`
			}

			rightBtn.onclick = function () {
				clickIndex ++
				if (clickIndex >= arryLen) {
					clickIndex = 0
				}
				bigImgDom.style.backgroundImage = `url(${ops.imgListArry[clickIndex]})`
			}
		},
		// 关闭事件
		closeShadowBox : function () {
			let closeBtn = document.getElementById(ops.shadowCloseBtn)
			let domBox = document.getElementById(ops.showdowBox)
			closeBtn.onclick = function () {
				chickImg = ''
				bigImgList = []
				domBox.style.display = 'none'
			}
		},
		// 提示语
		showToast : function (text) {
			if (text) {
				let tostHtml = document.createElement('div')
				tostHtml.setAttribute('id', 'toast')
				tostHtml.setAttribute('class', 'toast-tip')
				tostHtml.setAttribute('style', 'display:none')
				document.body.appendChild(tostHtml)
			}
			let dom = document.getElementById('toast')
			dom.innerHTML = text
			dom.style.display = 'block'
			setTimeout(function () { dom.style.display = 'none' }, 1500)
		}
	}
	win.setCreatDomFun = setCreatDomFun
})(window, document)

```