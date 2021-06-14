---
title: Javascript 数据基础类型, 浅拷贝深拷贝
date: '2019-05-21 15:05'
tag: 'Javascript'
---

### 基础数据类型
1、基本类型(值类型)：Boolean、String、Number、Symbol、Null、Undefined

2、引用类型：Function、Object、Array

特别说明：Symbol是ES6新增的原始数据类型，表示独一无二


- - -

### 引用，基础数据类型区别

1.**对于存放两种数据类型的变量** ：存放js基本数据类型的变量存放的是基本类型数据的实际值，而存放引用数据类型的变量是保存对它的引用即指针

2 **变量的交换（复制变量时）**： 对于存放基本数据类型的变量的交换，等于在一个新的作用域创建一个新的空间，新空间与原有的空间不会相互影响
对于存放引用数据类型的变量的交换，并不会创建一个新的空间，而是让对象或方法和之前对象或方法同时指向一个原有空间（即一个地址）

3 **声明变量时不同的内存分配**： 基本数据类型值（原始值）是存储在栈(stack)中的数据段，即直接存储在变量访问的位置
引用数据类型值是存储在堆（heap)中的对象即存储在变量处的值是一个指针，指向存储对象的内存地址。

4 **参数传递的不同** ： 首先要明确ECMAScript中所有的函数的参数都是按值来传递的
变量存储的基本类型的值只是把值传递给参数之后参数和这个变量互不影响
变量存储的引用数据类型值存储的是该引用值在堆内存中的内存地址，因此传递的值就是这个内存地址

> 举例说明
```javascript
var a = 8;
var b = a ;    //此处a和b有着相同的值，但是两者是相互独立的互不影响
a = 10;    //给a赋了新值，但是不影响b
console.log(b);    //输出8
```

```javascript
var c = [1,2,3];    //此处是将一个内存地址存储给了变量c
var d = c ;    //此处c和d指向的是同一个内存地址
c[0] = 2;    //此处将地址的值改变则会影响到d
console.log(d);    //[2,2,3]
```

```javascript
var e = [4,5,6];
var f = [7,8,9];    //在此处f和e指向了不同的内存地址，则两者将不会有任何相互影响
e[0] = 5;
console.log(f);    //[7,8,9]
```

- - -

### 浅拷贝和深拷贝

区分：假设B复制了A，当修改A时，看B是否会发生变化，如果B也跟着变了，说明这是浅拷贝，如果B没变，那就是深拷贝

场景：vue操作数据时，把item属性赋值给一个变量form，修改form时发现item也变了，这时候需要<kbd>form = { ...item }</kbd>

### 深入理解 -- 浅拷贝

**(1) Object.assign()**
```javascript
var obj1 = {a: 1, b: 2}
var obj2 = Object.assign({}, obj1) // 要注意Object.assign第一个参数必须是个空对象
 
obj2.a = 4
console.log(obj1, obj2)

// obj1, { a: 1, b: 2 }
// obj2, { a: 4, b: 2 }
```

**(2) 解构赋值**
```javascript
var obj1 = {a: 1, b: 2}
var obj2 = {...obj1}
 
obj2.a = 4
console.log(obj1, obj2)

// obj1, { a: 1, b: 2 }
// obj2, { a: 4, b: 2 }
```

缺陷， 这两种方式通常只能拷贝简单数据结构，如果数据结构复杂化类似于： 
```javascript
var obj1 = [{
    name: '臧三',
    childs: ['小明', '小芳']
}]
 
var obj2 = [...obj1]
obj2[0].childs = []
 
console.log(obj1, obj2)

// obj1, { name: '臧三', childs: [] }
// obj2, { name: '臧三', childs: [] }
```
发现两个obj的值都发生了改变，此时我们需要深拷贝

----------

### 深入理解 -- 深拷贝
**(1) 利用json.stringify**
```javascript
var obj1 = [{
    name: '臧三',
    childs: ['小明', '小芳']
}]
var obj2 = JSON.parse(JSON.stringify(obj1))
 
obj2[0].childs = []
console.log(obj1, obj2)

// obj1, { name: '臧三', childs: ['小明', '小芳'] }
// obj2, { name: '臧三', childs: [] }

注意：此方法适用于简单数据结构，如果数据结构包含undefined，function, 并不会被拷贝
```

**(2) 终极方法，递归实现**
```javascript
function extends (data) {
    if (!data) { return false }
    
    if( typeof data === 'object' ) {
        let items = typeof data.length === 'number' ? [] : {}
        for(let i in data) {
            items[i] = extend(data[i])
        }
        return items
    } else {
        return data
    }
}

var obj1 = [{
    name: '臧三',
    childs: ['小明', '小芳'],
    fn: function(){},
    age: undefined
}]
var obj2 = extends(obj1)

obj2[0].childs = []
console.log(obj1, obj2)

// obj1, { name: '臧三', childs: ['小明', '小芳'], age: undefined, fn: function(){} }
// obj2, { name: '臧三', childs: [], age: undefined, fn: function(){} }
```


