---
title: 改造：Vue-Quill-Editor 实现图片上传到服务器再插入富文本
date: '2019-06-19 09:09'
tag: 'Vue'
---

### 项目安装 
```javascript
    1. npm install vue-quill-editor    

    2. npm install quill 

```

### 配置使用
```javascript
// ===  Vue项目中Main.js中引入 ====
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'
Vue.use(VueQuillEditor)

```

### 代码展示
html
```html
  /**
   * @ editorWorld => 绑定富文本内容 
   * @ editorOption => 富文本编辑器配置(toolbar)
   * @ onEditorChange => 监听富文本变化
   * @ preview => 监听input上传事件获取file
   */
  
  // 富文本编辑器
  <quill-editor 
      v-model="editorWorld" 
      ref="myQuillEditor" 
      class="quillEditor" 
      :options="editorOption"                           
      @change="onEditorChange($event)">
  </quill-editor>

  // 自定义上传input
  <div v-show="false">
      <input type="file" ref="uploadAdd" id="uploadAdd" @change="preview($event)"/>
  </div>

```

javascript
```javascript
// 富文本编辑器的配置toorbar
const toolbarOptions = [
  ["bold", "italic", "underline", "strike"], // toggled buttons
  [{ header: 1 }, { header: 2 }], // custom button values
  [{ script: "sub" }, { script: "super" }], // superscript/subscript
  [{ indent: "-1" }, { indent: "+1" }], // outdent/indent
  [{ size: ["small", false, "large", "huge"] }], // custom dropdown
  [{ header: [1, 2, 3, 4, 5, 6, false] }],
  [{ color: [] }, { background: [] }], // dropdown with defaults from theme
  [{ font: [] }],
  [{ align: [] }],
  ["link", "image", "upload"],
  ["clean"] // remove formatting button
]; 

// export default => data 中的配置
stsData:"", // alioss临时授权
editorWorld: "",
editorOption: {
  modules: {
      toolbar: {
        container: toolbarOptions, // 工具栏
        handlers: {
          image: value => {
            if (value) {
                  // 触发上传事件
                  this.$refs.uploadAdd.click();
            } else {
                  this.quill.format("image", false);
            }
          },
          upload: value => {
            if (value) {
              alert("自定义文件上传");
            }
          }
        }
      }
    }
  }

```

到此，基本配置完毕，接下来开始上传(alioss)
```javascript
// 监听触发自定义上传文件
preview(event) {
  let files = document.getElementById("uploadAdd").files[0];
  // 调用上传方法处理
  this.beforeUpload(files);
  this.UploadTwo(files);
},

// 获取阿里OSS临时授权
getSts() {
  let OSS = require("ali-oss");
  return new Promise((ro, rj) => {
    apiSts().then(res => {
        this.stsData = res;
        ro();
    }).catch(err => { rj(err);});
  });
},

// 图片上传 -图片大小校验
beforeUpload(file) {
  const isJPG = file.type === "image/jpeg";
  const isLt2M = file.size / 1024 / 1024 < 2;
  if (!isJPG) {
    this.$message.error("请上传图 JPG 格式的图片!");
  }
  if (!isLt2M) {
    this.$message.error("图片大小不能超过 2M!");
  }
  return isJPG && isLt2M;
},

// 发现排期 - 图片上传
Upload(file) {
  // UUID，生成唯一的图片名称
  let uid = UUID.generate(); 
  let fileName = this.baseDIR + uid + ".jpg";

  // 获取alioss的认证信息
  let client = new OSS({
    region: "oss-cn-beijing",
    accessKeyId: this.stsData.accessKeyId,
    accessKeySecret: this.stsData.accessKeySecret,
    stsToken: this.stsData.securityToken,
    bucket: "test-yuansu"
  });
  
  // 上传文件
  client.put(fileName, file.file).then(result => {
      const allfile = result.url; // 获取图片路径
      // 更新富文本编辑框: 特别注意.quill，只有加上这个才能获取到富文本Obj对象
      this.$refs.myQuillEditor.quill.insertEmbed(
        this.$refs.myQuillEditor.quill.getSelection().index,"image",allfile
      );
  }).catch(err => {
      this.$message({message: err,type: "error"});
  });
},
```

- - -

### 知识拓展
```javascript
// 获取图片上传路径
getObjectURL(file) {
  let url = null;
  if (window.createObjectURL != undefined) {
    // basic
    url = window.createObjectURL(file);
  } else if (window.webkitURL != undefined) {
    // webkit or chrome
    url = window.webkitURL.createObjectURL(file);
  } else if (window.URL != undefined) {
    // mozilla(firefox)
    url = window.URL.createObjectURL(file);
  }
  console.log(url);
  return url;
}

```
