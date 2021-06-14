---
title: Vue + elementui 自定义指令防止重复请求 截流 防抖
date: '2019-06-30 11:20'
tag: ['Vue', 'ElementUi']
---

### 自定义指令
```javascript
// 局部directive的写法
directives:{
  noMoreClick: {
    inserted(el, binding) {
      el.addEventListener('click', () => {
        el.classList.add('is-disabled');
        el.disabled = true;
        setTimeout(() => {
          el.disabled = false;
          el.classList.remove('is-disabled');
        }, 3500)
      })
    }
  }
},

标签使用
<el-button size="small" @click="resetForm" v-no-more-click> Confirm </el-button>
```

### 防抖动
含义：点击时不会立即触发，2s没没有再次点击，则会执行函数体，如果2s内再次触发则取消当前事件重新计算时间；

理解：一个笑话：小明军训，教官发令：向左转！向右转！向后转！大家都照着做，唯有小明坐下来休息，教官火的一批，大声斥问他为啥不听指挥？小明说，我准备等你想好到底往哪个方向转，我再转； 放在笑话的语境里就是，只有教官最后一次发号后，小明才会转。

使用场景：input.change实时输入校验； window.resize窗口缩放完成后计算DOM尺寸等
```javascript
  // 函数防抖
  let timer = null;
  const btn = document.getElementById('btn-name')
  btn.onclick = () => {
    clearTimeout(timer);
    timer = setTimeout(()=>{
      console.log("执行买的http请求");
    },2000)
  };


  // 简易封装
  function debounce (fn, delay = 200) {
    let timeout;
    return function() {
      // 重新计时
      timeout && clearTimeout(timeout);
      timeout = setTimeout(fn.bind(this), delay, ...arguments);
    }
  }

  const handlerChange = debounce(function () {alert('更新触发了')})
  // 绑定监听
  document.querySelector("input").addEventListener('input', handlerChange);
```

### 函数节流
含义：假设不停点击状态下，每隔2s会触发一次函数体执行；

理解：学生上自习课，班主任五分钟过来巡视一次，五分钟内随便你怎么皮，房顶掀了都没事，只要你别在五分钟的时间间隔点上被班主任逮到，逮不到就当没发生，逮到她就要弄你了； 在这里，班主任就是节流器；你搞事，就是用户触发的事件；你被班主任逮住弄，就是执行回调函数。
```javascript
  // 函数节流
  let flag = true;
  const btn = document.getElementById('btn-name')
  btn.onclick = () => {
    if(!flag){ return false }
    flag = false;
    setTimeout(()=>{
      flag = true;
      console.log("执行买的http请求")
    },2000)
  };


  // 简易封装
  function throttle (fn, threshhold = 200) {
    let timeout;
    // 计算开始时间
    let start = new Date();
    return function () {
      // 触发时间
      const current = new Date() - 0;
      timeout && clearTimeout(timeout);
      // 如果到了时间间隔点，就执行一次回调
      if (current - start >= threshhold) {
        fn.call(this, ...arguments);
        // 更新开始时间
        start = current;
      } else {
        // 保证方法在脱离事件以后还会执行一次
        timeout = setTimeout(fn.bind(this), threshhold, ...arguments);
      }
    }
  }

  let handleMouseMove = throttle(function(e) {
    console.log(e.pageX, e.pageY);
  })

  // 绑定监听
  document.querySelector("#panel").addEventListener('mousemove', handleMouseMove);
```