---
title: Vue-transition + css（fade, top, bottom, left, right）
date: '2020-06-25'
tag: ['Vue', 'Css']
---

```
<template>
  <div>
    <transition name='fade'>
        <div v-if='goodSkuFlag' class='sku-shadow'></div>                 
    </transition>
    <transition name='bottom'>
      <div class='sku-list' v-if='goodSkuFlag'>
         // code here
      </div>
    </transition>
  </div>
</template>

```

```
.fade-enter-active, .fade-leave-active
  transition all .33s
.fade-enter, .fade-leave-to
  opacity 0

.left-enter-active, .left-leave-active
  transition all .33s
.left-enter, .left-leave-to
  opacity 0
  transform translateX(-100%)

.right-enter-active, .right-leave-active
  transition all .33s
.right-enter, .right-leave-to
  opacity 0
  transform translateX(100%)

.top-enter-active, .top-leave-active
  transition all .33s
.top-enter, .top-leave-to
  opacity 0
  transform translateY(-100%)

.bottom-enter-active, .bottom-leave-active
  transition all .33s
.bottom-enter, .bottom-leave-to
  opacity 0
  transform translateY(100%)

```