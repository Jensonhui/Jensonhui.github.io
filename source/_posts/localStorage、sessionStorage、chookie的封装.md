---
title: localStorage、sessionStorage、chookie的封装(设置过期时间)
date: '2019-08-11 19:14'
tag: 'Javascript'
---

### localStorage
```javascript
/**
 * localStorage
 * @调用：_local.set('access_token', '123456', 5000);
 * @调用：_local.get('access_token');
 */

var _local = {
  //存储,可设置过期时间
  set(key, value, expires) {
    let params = { key, value, expires };
    if (expires) {
      // 记录何时将值存入缓存，毫秒级
      var data = Object.assign(params, { startTime: new Date().getTime() });
      localStorage.setItem(key, JSON.stringify(data));
    } else {
      if (Object.prototype.toString.call(value) == '[object Object]') {
        value = JSON.stringify(value);
      }
      if (Object.prototype.toString.call(value) == '[object Array]') {
        value = JSON.stringify(value);
      }
      localStorage.setItem(key, value);
    }
  },
  //取出
  get(key) {
    let item = localStorage.getItem(key);
    // 先将拿到的试着进行json转为对象的形式
    try {
      item = JSON.parse(item);
    } catch (error) {
      // eslint-disable-next-line no-self-assign
      item = item;
    }
    // 如果有startTime的值，说明设置了失效时间
    if (item && item.startTime) {
      let date = new Date().getTime();
      // 如果大于就是过期了，如果小于或等于就还没过期
      if (date - item.startTime > item.expires) {
        localStorage.removeItem(name);
        return false;
      } else {
        return item.value;
      }
    } else {
      return item;
    }
  },
  // 删除
  remove(key) {
    localStorage.removeItem(key);
  },
  // 清除全部
  clear() {
    localStorage.clear();
  }
}
```

<br/>

### sessionStorage
```javascript
/**
* sessionStorage
*/
var _session = {
  get: function (key) {
    var data = sessionStorage[key];
    if (!data || data === "null") {
      return null;
    }
    return JSON.parse(data).value;
  },
  set: function (key, value) {
    var data = {
      value: value
    }
    sessionStorage[key] = JSON.stringify(data);
  },
  // 删除
  remove(key) {
    sessionStorage.removeItem(key);
  },
  // 清除全部
  clear() {
    sessionStorage.clear();
  }
}

export { _local, _session }
```

<br/>

### chookie
```javascript
/**
 * @description 保存cookie
 * @param {String} name 需要存储cookie的key
 * @param {String} value 需要存储cookie的value
 * @param {Number} timer 默认存储多少天
 */
function setCookie (name, value, timer = 1) {
  var Days = timer // 默认将被保存 1 天
  var exp = new Date()
  exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000)
  document.cookie = name + '=' + escape(value) + ';expires=' + exp.toGMTString()
}

/**
 * @description 获取cookie
 * @param {String} name 需要获取cookie的key
 */
function getCookie (name) {
  var arr = document.cookie.match(new RegExp('(^| )' + name + '=([^;]*)(;|$)'))
  if (arr != null) {
    return unescape(arr[2])
  } else {
    return null
  }
}

/**
 * @description 删除指定cookie
 * @param {String} name 需要删除cookie的key
 */
function delCookie (name) {
  var exp = new Date()
  exp.setTime(exp.getTime() - 1)
  var cval = getCookie(name)
  if (cval != null) document.cookie = name + '=' + cval + ';expires=' + exp.toGMTString()
}


/**
 * @description 清空cookie
 */
function clearCookie () {
  var keys = document.cookie.match(/[^ =;]+(?==)/g)
  var exp = new Date()
  exp.setTime(exp.getTime() - 1)
  if (keys) {
    for (var i = keys.length; i--;) {
      document.cookie = keys[i] + '=0;path=/;expires=' + exp.toUTCString() // 清除当前域名下的
      document.cookie = keys[i] + '=0;path=/;domain=' + document.domain + ';expires=' + exp.toUTCString() // 清除当前域名下的
      document.cookie = keys[i] + '=0;path=/;domain=ratingdog.cn;expires=' + exp.toUTCString() // 清除一级域名下的或指定的
    }
  }
}

export default { setCookie, getCookie, delCookie, clearCookie }

```

