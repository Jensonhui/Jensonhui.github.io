---
title: Javascript实现图片预览功能
date: '2017-10-21 10:04'
tag: 'Javascript'
---

```javascript
<img id="userHeaderImg" src="images/weChat.png" alt="" >
<input type="file" id="user-Uploadimg" onchange="xmTanUploadImg(this)" accept="image/*">

<script type="text/javascript">            
  if (typeof FileReader == 'undefined') {
      document.getElementById("xmTanDiv").InnerHTML = "<h1>当前浏览器不支持FileReader接口</h1>";
      document.getElementById("user-Uploadimg").setAttribute("disabled", "disabled");
  }

  // 选择图片，马上预览
  function xmTanUploadImg(obj) {
      var file = obj.files[0];               
      console.log(obj);console.log(file);
      console.log("file.size = " + file.size);
      var reader = new FileReader();
      // reader.onloadstart = function (e) {
      //     console.log("开始读取....");
      // }
      // reader.onprogress = function (e) {
      //     console.log("正在读取中....");
      // }
      // reader.onabort = function (e) {
      //     console.log("中断读取....");
      // }
      // reader.onerror = function (e) {
      //     console.log("读取异常....");
      // }
      reader.onload = function (e) {
        console.log("成功读取路径....");
        document.getElementById("userHeaderImg").src = e.target.result;
        //或者
        // document.getElementById("userHeaderImg").src = this.result;
      }
      reader.readAsDataURL(file)
  }
</script>
```