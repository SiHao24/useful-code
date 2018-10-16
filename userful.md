#### 1.获取元素
```javascript
  function getElem(selector) {
    if (selector[0] === '#') {
      return document.getElementById(selector.slice(1));
    } else {
      var node = document.querySelectorAll(selector);
      return node.length > 1 ? node : node[0]
    }
  }
```
#### 2.函数防抖
```javascript
  function debounce(fn, delay, now) {
    var timer;
    return function() {
      var self = this;
      if (now) {
        now = false;
        fn.call(self);
      }
      clearTimeout(timer);
      timer = setTimeout(function() {
        fn.call(self);
      }, delay)
    }
  }
```
#### 3.函数节流
```javascript
function throttle(fn, delay, now) {
  var timer, oldTime = new Data(), now;
  return function() {
    var self = this;
    if ((now = now Date()) - oldTime > time) {
      fn.call(self);
      oldTime = now;
    }
    clearTimeout(timer);
    timer = setTimeout(function() {
      fn.call(self);
    }, delay)
  }
}
```
#### 4.判断类型
```javascript
  function checkType(obj) {
    var type = typeof obj;
    if (type === 'object') {
      var objType = {}.toString.call(obj);
      return /\b(\w+)/.exec(objType)[1].toLowerCase();
    }
    return type;
  }
```
#### 5.获取url参数