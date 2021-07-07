---
title: 移动端的变革Rem
date: '2018-08-25 09:27'
tag: 'Css'
---

### 一、Rem是什么
rem（font size of the root element）是指相对于根元素的字体大小的单位。简单的说它就是一个相对单位。看到rem大家一定会想起em单位，em（font size of the element）是指相对于父元素的字体大小的单位。它们之间其实很相似，只不过一个计算的规则是依赖根元素一个是依赖父元素计算。


### 二、为什么要使用Rem
这里我特别强调web app，web page就不能使用rem吗，其实也当然可以，不过出于兼容性的考虑在web app下使用更加能突显这个单位的价值和能力。接下来我来看看目前一些企业的web app是怎么做屏幕适配的


### 三、Rem的使用方法
1 . 网页中的根元素指的是html我们通过设置html的字体大小就可以控制rem的大小, 例如：
```css
  html{	
      font-size:20px;
      // 根元素html的字体必须设置
  }
  .btn{
      width: 6rem;
      height: 3rem;
      line-height: 3rem;
      font-size: 1.2rem;  
  }
/* 
  推算出一下公式：
  10px  = 1rem 在根元素（font-size = 10px的时候）；

  20px  = 1rem 在根元素（font-size = 20px的时候）；

  40px  = 1rem 在根元素（font-size = 40px的时候）；
*/
```

<br/>

2 . 通过媒体查询设置兼容
```css
  @media only screen and (min-width: 401px){    
      html {font-size: 25px !important;}
  }
  @media only screen and (min-width: 428px){    
      html {font-size: 26.75px !important;}
  }
  @media only screen and (min-width: 481px){   
      html {font-size: 30px !important; 
  }}
  @media only screen and (min-width: 569px){    
      html {font-size: 35px !important; }
  }
  @media only screen and (min-width: 641px){    
      html { font-size: 40px !important; }
  }
```

<br/>

3 . 通过Javascript动态的设置【推荐】
```javascript
  a : 方法一
  (function (docs, win) {
    var docEls = docs.documentElement,
    resizeEvts = 'orientationchange' in window ? 'orientationchange' : 'resize',
    recalcs = function () {
        //getBoundingClientRect()这个方法返回一个矩形对象
        window.rem = docEls.getBoundingClientRect().width/25;
        docEls.style.fontSize = window.rem + 'px';
    };
    recalcs();
    if (!docs.addEventListener) return;
    win.addEventListener(resizeEvts, recalcs, false);
  })(document, window);


  b : 方法二
  document.documentElement.style.fontSize = document.documentElement.clientWidth / 750*100 + 'px';
  // 750是设计尺寸， 可替换成手机PSD设计尺寸大小

    
  c : 方法三 (阿里某大牛)
  (function (doc, win) {
    const docEl = doc.documentElement
    const resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize'
    const recalc = function () {
      const clientWidth = docEl.clientWidth
      if (!clientWidth) return
      const max = 24
      const min = 9.3125
      let size = 20 * (clientWidth / 320)

      size = Math.min(size, max)
      size = Math.max(size, min)
      docEl.style.fontSize = size + 'px'
    }
    if (!doc.addEventListener) return
    win.addEventListener(resizeEvt, recalc, false)
    doc.addEventListener('DOMContentLoaded', recalc, false)
  })(document, window)

  d : 方法
  (function () {
    const width = document.documentElement.clientWidth || document.body.clientWidth
    const ratio = width / 375
    const fonts = 100 * ratio
    document.querySelector('html').style.fontSize = fonts + 'px'
  })()

```

