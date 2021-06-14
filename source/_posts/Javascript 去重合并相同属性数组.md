---
title: Javascript 去重合并具有相同属性的数组
date: '2019-07-11 17:24'
tag: 'Javascript'
---

### 原始数据
```javascript
// oldArry
{
  "modelist": [
    {
      "parentid": 1, // 父级id
      "menuItem": [
        {
          "id": 12,
          "name": "测试数据一"
        }
      ]
    },
    {
      "parentid": 1, // 父级id
      "menuItem": [
        {
          "id": 13,
          "name": "测试数据二"
        }
      ]
    }
  ]
}
```

上面JSON中，相同父级ID的有2组子数据，我们需要把这两个合并成一个，即：
```javascript
{
  "modelist": [
    {
      "parentid": 1, // 父级id
      "menuItem": [
        {
          "id": 12,
          "name": "测试数据一"
        },
        {
          "id": 13,
          "name": "测试数据二"
        }
      ]
    }
  ]
}
```

### 实现过程
```javascript
// 定义一个newArry
let menuModelList = [];  
// 定义空对象用于合并
let menuItem= {}; 
// 去重合并
oldArry.forEach((el, i) => {
  if (!menuItem[el.parentid]) {
    menuModelList.push(el);
    menuItem[el.parentid] = true;
  } else {
    menuModelList.forEach(el => {
      if (el.parentid === oldArry[i].parentid) {
        el.menuItem= el.menuItem.concat(oldArry[i].menuItem);
        // ES6 语法
        el.menuItem= [...el.menuItem, ...oldArry[i].menuItem];
      }
    });
  }
});

```