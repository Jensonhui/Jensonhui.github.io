---
title: Vue + elementUi 实现table上移、下移
date: '2019-06-25 18:58'
tag: ['Vue', 'ElementUi']
---


### 实现思路
实现背景，table没有分页，这是前提！！

1 . 如果后台有接口，直接改动完把列表list提交给后端

2 . 如果没有后台，增加权重字段，提交到后端

3 . 上下移动，实际就是操作数组list的顺序index

<br/>

### Javascript
```javascript
// ================ data ================
  HotTableList:[]

// ================ html ================
<template v-slot="scope">
    <el-button  @click="upindex(scope.$index,scope.row)">上移</el-button>
    <el-button  @click="downindex(scope.$index,scope.row)">下移</el-button>
</template>

// ================ 上移 ===================
upindex(index, row) {
  if (index > 0) {
    let upDate = this.HotTableList[index - 1];
    this.HotTableList.splice(index - 1, 1);
    this.HotTableList.splice(index, 0, upDate);
  } else {
    this.$message.warning("已经是第一条了！");
    return false;
  }
},

// ================ 下移 ===================
downindex(index, row) {
  if (index + 1 === this.HotTableList.length) {
    this.$message.warning("已经是最后一条了！");
    return false;
  } else {
    let downDate = this.HotTableList[index + 1];
    this.HotTableList.splice(index + 1, 1);
    this.HotTableList.splice(index, 0, downDate);
  }
},
    
```
