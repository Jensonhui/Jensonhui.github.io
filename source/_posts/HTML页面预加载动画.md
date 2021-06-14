---
title: HTML页面预加载 (动效非功能)
date: '2018-03-16 23:18'
tag: ['Javascript', 'Css']
---

### 什么是预加载
页面资源预加载(Link prefetch)是浏览器提供的一个技巧，目的是让浏览器在空闲时间下载或预读取一些文档资源，用户在将来将会访问这些资源。一个Web页面可以对浏览器设置一系列的预加载指示，当浏览器加载完当前页面后，它会在后台静悄悄的加载指定的文档，并把它们存储在缓存里。当用户访问到这些预加载的文档后，浏览器能快速的从缓存里提取给用户。

**简单的来讲就是**：让浏览器预先加载用户访问当前页后极有可能访问的其他资源(页面，图片，视频等)
演示地址：http://crusader12.com/C12HoverAlls/

### 使用方法
背景颜色可以替换成自己的页面主色调

1 . 引入以下css样式文件
```css
.chromeframe{margin:.2em 0;padding:.2em 0;background:#ccc;color:#000;}
#loader-wrapper{position:fixed;top:0;left:0;z-index:999999;width:100%;height:100%;}
#loader{position:relative;top:50%;left:50%;z-index:1001;display:block;margin:-75px 0 0 -75px;width:150px;height:150px;border:3px solid transparent;border-radius:50%;border-top-color:#FFF;-webkit-animation:spin 2s linear infinite;-ms-animation:spin 2s linear infinite;-moz-animation:spin 2s linear infinite;-o-animation:spin 2s linear infinite;animation:spin 2s linear infinite;}
#loader:before{position:absolute;top:5px;right:5px;bottom:5px;left:5px;border:3px solid transparent;border-radius:50%;content:"";border-top-color:#FFF;-webkit-animation:spin 3s linear infinite;-moz-animation:spin 3s linear infinite;-o-animation:spin 3s linear infinite;-ms-animation:spin 3s linear infinite;animation:spin 3s linear infinite;}
#loader:after{position:absolute;top:15px;right:15px;bottom:15px;left:15px;border:3px solid transparent;border-radius:50%;content:"";border-top-color:#FFF;-moz-animation:spin 1.5s linear infinite;-o-animation:spin 1.5s linear infinite;-ms-animation:spin 1.5s linear infinite;-webkit-animation:spin 1.5s linear infinite;animation:spin 1.5s linear infinite;}
@-webkit-keyframes spin{0%{-webkit-transform:rotate(0);transform:rotate(0);-ms-transform:rotate(0);}
100%{-webkit-transform:rotate(360deg);transform:rotate(360deg);-ms-transform:rotate(360deg);}
}
@keyframes spin{0%{-webkit-transform:rotate(0);transform:rotate(0);-ms-transform:rotate(0);}
100%{-webkit-transform:rotate(360deg);transform:rotate(360deg);-ms-transform:rotate(360deg);}
}
#loader-wrapper .loader-section{position:fixed;top:0;z-index:1000;width:51%;height:100%;background-color:#2095f2;-webkit-transform:translateX(0);transform:translateX(0);-ms-transform:translateX(0);}
#loader-wrapper .loader-section.section-left{left:0;}
#loader-wrapper .loader-section.section-right{right:0;}
.loaded #loader-wrapper .loader-section.section-left{-webkit-transition:all .7s .3s cubic-bezier(.645,.045,.355,1);transition:all .7s .3s cubic-bezier(.645,.045,.355,1);-webkit-transform:translateX(-100%);transform:translateX(-100%);-ms-transform:translateX(-100%);}
.loaded #loader-wrapper .loader-section.section-right{-webkit-transition:all .7s .3s cubic-bezier(.645,.045,.355,1);transition:all .7s .3s cubic-bezier(.645,.045,.355,1);-webkit-transform:translateX(100%);transform:translateX(100%);-ms-transform:translateX(100%);}
.loaded #loader{opacity:0;-webkit-transition:all .3s ease-out;transition:all .3s ease-out;}
.loaded #loader-wrapper{visibility:hidden;-webkit-transition:all .3s 1s ease-out;transition:all .3s 1s ease-out;-webkit-transform:translateY(-100%);transform:translateY(-100%);-ms-transform:translateY(-100%);}
.no-js #loader-wrapper{display:none;}
.no-js h1{color:#222;}
#loader-wrapper .load_title{position:absolute;top:60%;z-index:9999999999999;width:100%;color:#FFF;text-align:center;font-size:19px;font-family:'Open Sans';line-height:30px;opacity:1;}
#loader-wrapper .load_title span{color:#FFF;font-weight:400;font-style:italic;font-size:13px;opacity:.5;}

```

2 . 在head标签下写入以下Javascript代码
```javascript
<script>
    //当页面加载状态改变的时候执行这个方法.
    document.onreadystatechange = subSomething;
    function subSomething() { 
        //当页面加载状态为完全结束时进入 
        if(document.readyState == "complete"){ 
            document.getElementsByTagName('body')[0].className = "loaded";
            document.getElementsByClassName('load_title')[0].remove();
        }
    }
</script>
```

3 . 在body中写入以下html代码
```html
<div id="loader-wrapper">
    <div id="loader"></div>
    <div class="loader-section section-left"></div>
    <div class="loader-section section-right"></div>
    <div class="load_title">正在加载CLARIVATE站点</div>
</div>
```

