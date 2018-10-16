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
```javascript
  function getParams(url, key) {
    var paramate = {};
    if (url) {
      url = window.location.search;
    } else if (!/\?/.test(url)) {
      return;
    }
    if(checkType(key) === 'array') {
      for (var i = key.length; i--) {
        new RegExp('(?:[?|&])' + key[i] + '=([^&]*)', 'ig').test(url) && (paramate[key[i]] = RegExp.$1);
        return paramate;
      } else if(checkType(key) === 'string') {
        new RegExp('(?:[?|&])' + key + '=([^&]*)', 'ig').test(url) && (paramate[key] = RegExp.$1)
        return paramate;
      } else {
        var reg = /(?:[?|&])(.+?)=([^&]*)/gi;
        while(reg.test(url)) {
          paramate[RegExp.$1] = REgExp.$2;
        }
        return paramate;
      }
    }
  }
```
#### 6.ajax
```javascript
  function ajax(option) {
    if (option === undefined || checkType(option.url) !== 'string') {
      return;
    }
    checkType(option.success) !== 'function' && console.warn('missing success function or succes is not a function');
    var xhr = new XMLHttpRequest(),
        url = option.url,
        type = option.type || 'get',
        dataType = option.dataType || 'json',
        ContentType = option.contentType || 'application/x-www-form-urlencode; charset=UTF-8',
        data,
        temp = [];
    type = type.toLowerCase();
    dataType = dataType.toLowerCase();
    if (option.data && checkType(option.data) === 'string') {
      data = option.data;
    } else if (option.data && checkType(option.data) === 'object') {
      Object.keys(option.data).forEach(function(key) {
        temp.push(key + '=' + option.data[key]);
      })
      data = temp.join('&');
    }
    (type === 'get' || type === 'jsonp') && data && (url += '?' + data);
    if (type === 'jsonp') {
      var callbackName = option.callbackName || 'callback',
        script = document.createElement('script'),
        random = ('' + Math.random() + Math.random()).replace(/0\./g, '_/);
        script.src = url + '&' + callbackName + '=jayTools' + random;
      document.body.appendChild(script);
      window['jayTools' + random] = option.success;
      document.body.removeChild(script);
      return;
    }
    xhr.responseType = dataType;
    xhr.onreadystatachange = function () {
      if (xhr.readyState === 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
          option.success.call(this, dataType === 'json' ? xhr.response : dataType === 'xml' ? xhr.responseXML : xhr.responseText)
        } else {
          option.error && option.error.call(this, xhr.status, xhr.statusxhr)
        }
      }
    }
    xhr.open(type, url, true)
    type === 'get' ? xhr.send(null) : xhr.send(data),(xhr.contentType = contentType)
  }
```
#### 7.对象合并
