---
title: Javascript获取图片路径(兼容)
date: '2017-09-20 10:44'
tag: 'Javascript'
---

```javascript
<h1>JS获取文件域完整路径的方法，兼容不同浏览器</h1>
<div id="text" style="color:#f00;"></div>
<input type="file" id="file" onchange="getvl(this)" />


//判断浏览器
function getvl(obj){
    var Sys = {}; 
    var ua = navigator.userAgent.toLowerCase(); 
    var s; 
    (s = ua.match(/msie ([\d.]+)/)) ? Sys.ie = s[1] : 
    (s = ua.match(/firefox\/([\d.]+)/)) ? Sys.firefox = s[1] : 
    (s = ua.match(/chrome\/([\d.]+)/)) ? Sys.chrome = s[1] : 
    (s = ua.match(/opera.([\d.]+)/)) ? Sys.opera = s[1] : 
    (s = ua.match(/version\/([\d.]+).*safari/)) ? Sys.safari = s[1] : 0;
    var file_url="";
    if(Sys.ie<="6.0"){
        //ie5.5,ie6.0
        file_url = obj.value;
    }else if(Sys.ie>="7.0"){
        //ie7,ie8
        obj.select();
        file_url = document.selection.createRange().text;
    }else if(Sys.firefox){
        //fx
        //获取的路径为FF识别的加密字符串
        //file_url = document.getElementById("file").files[0].getAsDataURL(); 
        file_url = readFileFirefox(obj);
    }else if(Sys.chrome){
        file_url = obj.value;
    }
    //alert(file_url);
    document.getElementById("text").innerHTML="获取文件域完整路径为："+ file_url;
}
```