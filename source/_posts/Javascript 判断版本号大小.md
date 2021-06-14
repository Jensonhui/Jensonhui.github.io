---
title: Javascript判断版本号大小
date: '2020-04-09'
tag: 'Javascript'
---

场景: 移动端H5

说明: 历时版本原生会存到HTTP请求代理头部


//  获取历史版本,用户APP当前版本
```
备注: duiyu_(android/ios)是原生存入的标识


let tem = null // 版本号 
let PhoneModel = null // 手机标识
let oldVersion = null // 全局历史版本变量
let ua = navigator.userAgent.toLowerCase() // 获取用户代理属性duiyu_ios/1.9.3
if (ua.indexOf('_android') !== -1) { PhoneModel = 'duiyu_android' }
if (ua.indexOf('duiyu_ios') !== -1) { PhoneModel = 'duiyu_ios' }
switch (PhoneModel) {
  case 'duiyu_android':
    tem = ua.match(/duiyu_android\/[\d.]+/gi).toString()
    break
  case 'duiyu_ios':
    tem = ua.match(/duiyu_ios\/[\d.]+/gi).toString()
    break
}
oldVersion = tem ? tem.split('/')[1] : null // 赋值全局变量
```

// 比较版本 (假定字符串的每节数都在5位以下: 2.1.0)
```
function checkVersion (num) {
  let visonA = num.toString()
  let visonB = visonA.split('.')
  let numPlace = ['', '0', '00', '000', '0000']
  let visonC = numPlace.reverse()
  for (let i = 0; i < visonB.length; i++) {
    let len = visonB[i].length
    visonB[i] = visonC[len] + visonB[i]
  }
  let res = visonB.join('')
  return res
}
```


[参考链接: ][1]


======================== 4月20号 更新  ===============================
懒人专属 ,  没找到合适的, 就自己花时间写了一个,

**性能待测试  性能待测试  性能待测试  ! ! ! !**

**备注声明 :**  
> 1 . 默认规定末尾含字母的形式 b > a ; 

> 2 . 暂时不支持单个带字母形式如 : 1.8.1  1.8.1a  或者  1.8.1   1.8b 或者  v1.8.1  1.8.2b;


```
const olds = '1.8.1'
const news = '1.8.3'
let getVersion = {
	splitVersion : function (oldval, newval) {
		// 校验是否传入参数
		if (!oldval || !newval) { console.error('[传入参数不完整!]'); return false }
 
		// 校验参数是否为string
		if (typeof oldval !== 'string' || typeof newval !== 'string') {console.error('[请传入字符串类型!]'); return false }
 
		// 检测字母
		const alpA = this.checkAlphabet(oldval)
		const alpB = this.checkAlphabet(newval)
 
		// 过滤掉版本字母
		let version = null
		let visA = oldval.split(/\.|\D/)
		let visB = newval.split(/\.|\D/)
 
		// 含字母时重组, 格式需固定
		// 不允许出现单个字母形式1.2.1 | 1.2.1a
		if (alpA && alpB) {
			visA[visA.length - 1] = alpA
			visB[visB.length - 1] = alpB
		}
 
		this.replaceArry(visA)
		this.replaceArry(visB)
		this.coverZero(visA, visB)
 
		version = this.checkVersion(visA, visB, oldval, newval)
		console.log(version)
	},
	// 比较版本
	checkVersion: function (visA, visB, oldval, newval) {
		for (let i in visA) {
			for (let j in visB) {
				if (i === j) {
					if (parseInt(visA[i]) < parseInt(visB[j])) { return '最新版本 : ' + newval }
					if (parseInt(visA[i]) > parseInt(visB[j])) { return '已经最新 : ' + oldval }
				}
			}
		}
		return '已经最新 : ' + oldval
	},
	// 替换空元素
	replaceArry : function (item) {
		for (let i in item) {
			if (item[i] === '') { item[i] = "0" }
		}
	},
	// 补全数组长度
	coverZero: function (visA, visB) {
		const lenA = visA.length
		const lenB = visB.length
		if (lenA === lenB) { return false }
		const minVal = lenA > lenB ? lenB : lenA
		const maxVal = lenA > lenB ? lenA : lenB
		const aryVal = lenA > lenB ? visB : visA
		for (let i = minVal; i < maxVal; i++) { aryVal.push("0") }
	},
	// 校验字母
	checkAlphabet: function (item) {
		let reg = /^[A-Za-z]+$/;
		let len = item.length - 1
		let lastNode = item[len]
		if (reg.test(lastNode)) {
			// 转化, 规定默认b > a
			return item.charCodeAt(len).toString()
		}
	}
}
// 调用
getVersion.splitVersion(olds, news)

```




  [1]: https://www.cnblogs.com/zengnificant/p/5634389.html