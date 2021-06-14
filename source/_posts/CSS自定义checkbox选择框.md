---
title: CSS自定义checkbox选择框 + JS实现全选 取消 删除
date: '2018-01-15 11:24'
tag: ['Css', 'Javascript']
---

### 实现效果 + 思路
1 . 点击全选，所有checkbox为选中状态 checked = true;

2 . 全选状态下，单机input[type="checkbox"]，取消选择； 每次操作更新 全选状态(选择checkbox的个数)；
![input=checkbox][1]

&nbsp;
```html
<input type="button" id="checkboxall-btn" value="全选">
<input type="checkbox" id="checkbox-all" name="ckbx"><label for="checkbox-all"></label>
<input id="deleteAll" type="button" value="删除选中名片">
```


```css
.table input[type="checkbox"]{
    display: none;
}
.table input[type="checkbox"] ~ label{
    display: inline-block;
    width: 18px;
    height: 18px;
    color: #fff;
    margin-right:5px;
    cursor: pointer;
    line-height: 18px;
    text-align: center;
    border-radius:5px;
    background-color: #cccccc;
}
.table input[type="checkbox"]:checked + label {
    background-color: #0067D0;
}
.table input[type="checkbox"] + label:before{
    content:"\2714";
}

```



```javascript
/* 选择全部、取消选择 */
function checkAllBox(){
    // 获取全选按钮
    var checkBtn = document.getElementById('checkboxall-btn');
    checkBtn.onclick = function (){
        // 获取checkbox
        var items = document.getElementsByName("ckbx");
        if(checkBtn.checked){  
            for(var i =0 ; i < items.length ; i++){  
                items[i].checked = true;  
            }  
        }else{  
            for(var i =0 ; i < items.length ; i++){  
                items[i].checked = false; 
            }  
        } 
    }
}


/* 单个选择取消选择 */
function checkOneBox(){
    var items = document.getElementsByName("ckbx");
    for (var i = 0; i < items.length; i++) {
        items[i].onclick = function() {
            var number = 0;
            for (var j = 0; j < items.length; j++) {
                if (items[j].checked) {
                    number++;
                }
            }
            document.getElementById("checkboxall-btn").checked = (items.length == number);
        }
    }
}


/* 全选、选中删除 */
function deleteAll(){
    // 获取delete按钮和checkbox数量
    var deleteBtn = document.getElementById('deleteAll');
    var items = document.getElementsByName("ckbx");
    deleteBtn.onclick = function(){
        // 采用递减删除(由高到低)， 反之会出现删除不干净问题
        for(var i = items.length-1; i >= 0; i--){
            items[i].i = i;
            if(items[i].checked == true){
                // 查询父元素
                items[i].parentNode.parentNode.remove();
            }
        }
    }
    
}

window.onload = function(){checkAllBox();checkOneBox();deleteAll();}
```

<br/>

### 实现( ES6箭头函数 )
```javascript
<form action="#">
    <input type="checkbox" value="1"><br>
    <input type="checkbox" value="2"><br>
    <input type="checkbox" value="3"><br>
    <input type="checkbox" value="4"><br>
    <button id="all">全选</button>
    <button id="del">删除</button>
</form>    
    

<script>
    var input = document.querySelectorAll('input');
    var len = input.length;
    var btn = document.getElementById('del');
    var allbtn = document.getElementById('all');
    allbtn.onclick = function(){
        input.forEach((item)=>{
            if(item.checked ==true){
                item.checked = false
            }
            else{
                item.checked = true
            }
        })
    }
    btn.onclick = function(){
        input.forEach((item,index)=>{
            if(item.checked){
                item.remove()
            }
        })
    }
</script>
```

<br/>


### Vue 实现单选，全选
```javascript
// 单选

checkOnly (item) {
  let flag = true  // 这里是关键
  const listbox = this.carList
  item.isSlect = !item.isSlect
  for (let i = 0; i < listbox.length; i++) {
  if (listbox[i].isSlect === false) { flag = false }
  }
  flag ? this.isCheckAll = true : this.isCheckAll = false
}

```


```javascript
// 多选
chickAllOption () {
  const listbox = this.carList
  if (this.isCheckAll) {
    for (let i = 0; i < listbox.length; i++) {
        listbox[i].isSlect = false
    }
  } else {
    for (let i = 0; i < listbox.length; i++) {
      listbox[i].isSlect = true
    }
  }
}
```


  [1]: http://blog.jensonhui.top/checkBox.gif