---
title: Vue 仿京东APP添加购物车抛物线动效
date: '2019-09-19'
tag: 'Vue'
---

吐槽: 买菜确实用不到高数, 但是学好高数对工作真的会有用 ! ! ! 悔不当初.

### 知识点
1 . Vue提供transtion动画 + Javascript动画生命钩子函数实现动效
```
v-on:before-enter="beforeEnter" （动画进入前）
v-on:enter="enter"（动画进入时）
v-on:after-enter="afterEnter" （动画进入完后）
```
2 . 贝塞尔曲线 
```
 // css
 transition: all .5s cubic-bezier(0.49, 0.15, 0.65, 0.2)
```

3 . 抛物线动画
传送门=>  << [小折腾：JavaScript与元素间的抛物线轨迹运动][1] >>

###  方式一: Vue
```
<div id="app">
  <ul class="shop">
    <li @click="shout($event)"> 添加 </li>
  </ul>
  <div class="addchange" @click="addToCar">加入购物车</div>
  <div class="cart">{{count}}</div>
  <div class="ball-container">
    <div v-for="ball in balls">
      <transition name="drop" @before-enter="beforeEnter" @enter="Enter" @after-enter="afterEnter">
        <div class="ball" v-show="ball.show">
          <div class="inner inner-hock">
            <img :src="srcName">
          </div>
        </div>
      </transition>
    </div>
  </div>
</div>

```

```
*{margin:0;padding:0}
#app{width:100%;height:100vh;font-size:30px}
.shop{position:fixed;top:30%;left:50%;font-size:3rem}
.addchange{position:fixed;bottom:1rem;right:2rem;z-index:10;font-size:6rem;text-align:center;color:orange}
.ball{position:fixed;left:1rem;bottom:1rem;z-index:200;transition:all .5s cubic-bezier(.49,.15,.65,.2)}
.inner{width:8rem;height:8rem;overflow:hidden;border-radius:50%;background-color:transparent;transition:all .5s linear}
.inner img{position:abaolute;top:0;left:0;z-index:30;width:100%;height:100%;border-radius:50%}
.cart{position:fixed;bottom:2rem;left:3rem;width:5rem;height:5rem;line-height:5rem;text-align:center;background-color:orange;color:#fff}
```

```javascript
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
new Vue({
	el: "#app",
	data: {
		count: 0,
		srcName: "https://www.w3school.com.cn/i/eg_tulip.jpg",
		balls: [{
			show: false
		}, {
			show: false
		}, {
			show: false
		}, ],
		dropBalls: [],
		flage: false,
	},
	mounted() {
		this.flage = true
	},
	methods: {
		addToCar() {
			if (this.flage) {
				this.shout()
			}
		}, shout() {
			this.dropBall();
			this.count++
		}, dropBall(el) {
			for (let i = 0; i < this.balls.length; i++) {
				let ball = this.balls[i];
				if (!ball.show) {
					ball.show = true;
					this.dropBalls.push(ball);
					return
				}
			}
		}, beforeEnter(el) {
			let count = this.balls.length;
			while (count--) {
				let ball = this.balls[count];
				if (ball.show) {
					let y = -(window.innerHeight - window.innerWidth / 2 - 120);
					let x = window.innerWidth / 2 - 60;
					el.style.display = '';
					el.style.webkitTransform = 'translateY(' + +'px)';
					el.style.transform = 'translateY(' + y + 'px)';
					let inner = el.getElementsByClassName('inner-hock')[0];
					inner.style.webkitTransform = 'translateX(' + x + 'px)';
					inner.style.transform = 'translateX(' + x + 'px)'
				}
			}
		}, Enter(el, done) {
			setTimeout(function() {
				let rf = el.offsetHeight;
				el.style.webkitTransform = 'translate3d(0,0,0)';
				el.style.transform = 'translate3d(0,0,0)';
				let inner = el.getElementsByClassName('inner-hock')[0];
				inner.style.webkitTransform = 'translate3d(0,0,0)';
				inner.style.transform = 'translate3d(0,0,0)';
				inner.style.transform = `scale(0.5)`;
				el.addEventListener('transitionend', done)
			}, 190)
		}, afterEnter(el) {
			let ball = this.dropBalls.shift();
			if (ball) {
				ball.show = false;
				el.style.display = 'none'
			}
		}
	}
})
</script>
```


GitHub : [vue仿京东APP添加购物车抛物线动画][2]


----------


### 方式二: 原生JS
```html
<html>
<head>
    <meta charset="UTF-8">
    <title>JS,添加购物车抛物线</title>
</head>
<style>
* {
  padding: 0;
  margin: 0;
}
#ball {
  width:12px;
  height:12px;
  background: #5EA345;
  border-radius: 50%;
  position: fixed;
  transition: left 1s linear, top 1s ease-in;
}
</style>
<body style="width:100%;height:100%;">
  <div id="ball"></div>
  <script>
    var $ball = document.getElementById('ball');
    document.body.onclick = function (evt) {
    console.log(evt.pageX,evt.pageY)
    $ball.style.top = evt.pageY+'px';
    $ball.style.left = evt.pageX+'px';
    $ball.style.transition = 'left 0s, top 0s';
      setTimeout(()=>{
        $ball.style.top = window.innerHeight+'px';
        $ball.style.left = '0px';
        $ball.style.transition = 'left 1s linear, top 1s ease-in';
      }, 20)
    }
  </script>
</body>
</html>
```

  [1]: https://www.zhangxinxu.com/wordpress/2013/12/javascript-js-%E5%85%83%E7%B4%A0-%E6%8A%9B%E7%89%A9%E7%BA%BF-%E8%BF%90%E5%8A%A8-%E5%8A%A8%E7%94%BB/
  [2]: https://github.com/Jensonhui/vue-jd-shopcar