## 3.2 高阶函数是指至少满足下列条件之一的函数

- 函数可以作为参数被传递
- 函数可以作为返回值输出

### 3.2.3 函数节流

```js
let throttle = function (fn, interval) {
  let _self = fn,
    timer,
    firstTime = true;

  return function () {
    let args = arguments;
    _me = this;

    if (firstTime) {
      _self.apply(_me, args);
      return (firstTime = false);
    }

    if (timer) {
      return false;
    }

    timer = setTimeout(function () {
      clearTimeout(timer);
      timer = null;
      _self.apply(_me, args);
    }, interval || 500);
  };
};

window.onresize = throttle(function () {
  console.log(123);
});
```

### 3.2.4 分时函数

```js
let timeChunk = function (ary, fn, count) {
  let obj, t;

  let len = ary.length;

  let start = function () {
    for (let i = 0; i < Math.min(count || 1, ary.length); i++) {
      let obj = ary.shift();
      fn(obj);
    }
  };

  return function () {
    t = setInterval(function () {
      if (ary.length === 0) {
        // 全部节点创建完毕
        return clearInterval(t);
      }
      start();
    }, 200);
  };
};

let ary = [];

for (let i = 0; i < 1000; i++) {
  ary.push(i);
}

let renderFriendList = timeChunk(
  ary,
  function (n) {
    let div = document.createElement("div");
    div.innerHTML = n;
    document.body.appendChild(div);
  },
  8
);

renderFriendList();
```

## 4 单例模式

```js
// 管理单例职责
let getSingle = function (fn) {
  let result;
  return function () {
    return result || (result = fn.apply(this, arguments));
  };
};

// 创建对象职责
let createLoginLayer = function () {
  let div = document.createElement("div");
  div.innerHTML = "我是登录弹框";
  div.style.display = "none";
  document.body.appendChild(div);
  console.log(333);
  return div;
};

let createSingleLoginLayer = getSingle(createLoginLayer);

document.getElementById("loginBtn").onclick = function () {
  let abc = createSingleLoginLayer();
  abc.style.display = "block";
};

// 创建对象和管理单例的职责被分布在两个不同的方法，组合起来才是最合适的单例模式
```

## 5.6 表单验证

## 5.8 策略模式计算奖金

```js
// strategies 策略模式计算奖金

let S = function (salary) {
  return salary * 4;
};

let A = function (salary) {
  return salary * 3;
};

let B = function (salary) {
  return salary * 2;
};

let calculateBonus = function (func, salary) {
  return func(salary);
};

console.log("123", calculateBonus(S, 10000)); // 40000
```

## 6.6 虚拟代理合并 http 请求

```html
<div>
  <input type="checkbox" id="1" />1 <input type="checkbox" id="2" />2
  <input type="checkbox" id="3" />3 <input type="checkbox" id="4" />4
  <input type="checkbox" id="5" />5 <input type="checkbox" id="6" />6
  <input type="checkbox" id="7" />7 <input type="checkbox" id="8" />8
  <input type="checkbox" id="9" />9
</div>
```

```js
let checkbox = document.getElementsByTagName("input");

let synchronousFile = function (id) {
  console.log(`开始同步id为：${id}`);
};

let proxySynchronousFile = (function () {
  let cache = [],
    timer;

  return function (id) {
    cache.push(id);
    if (timer) {
      return;
    }
    timer = setTimeout(function () {
      synchronousFile(cache.join(","));
      clearTimeout(timer);
      timer = null;
      cache.length = 0;
    }, 2000);
  };
})();

for (let i = 0; i < checkbox.length; i++) {
  let c = checkbox[i];
  c.onclick = function () {
    if (this.checked === true) {
      proxySynchronousFile(this.id);
    }
  };
}
```

## 8.6 发布订阅模式

```html
<div>
  <button id="count">点我</button>
  <div id="show"></div>
</div>
```

```js
let Event = (function () {
  let clientList = {},
    listen,
    trigger,
    reomve;
  listen = function (key, fn) {
    if (!clientList[key]) {
      clientList[key] = [];
    }
    clientList[key].push(fn);
  };
  trigger = function () {
    let key = Array.prototype.shift.call(arguments),
      fns = clientList[key];
    if (!fns || fns.length === 0) {
      return false;
    }
    for (let i = 0, fn; (fn = fns[i++]); ) {
      fn.apply(this, arguments);
    }
  };
  reomve = function (key, fn) {
    let fns = clientList[key];
    if (!fns) {
      return false;
    }
    if (!fn) {
      fns && (fns.length = 0);
    } else {
      for (let l = fns.length - 1; l >= 0; l--) {
        let _fn = fns[l];
        if (_fn === fn) {
          fns.splice(l, 1);
        }
      }
    }
  };

  return {
    listen,
    trigger,
    reomve,
  };
})();

console.log("=================  取消订阅  ==============");

Event.listen(
  "square88",
  (fn1 = function (price) {
    console.log("价格1" + price);
  })
);

Event.listen(
  "square88",
  (fn2 = function (price) {
    console.log("价格2" + price);
  })
);

Event.reomve("square88", fn1);

Event.trigger("square88", 2000000);

console.log("================ 模块间通信 ===============");

let count = 0;

let a = (function () {
  document.getElementById("count").onclick = function () {
    Event.trigger("add", count++);
  };
})();

let b = (function () {
  let div = document.getElementById("show");
  Event.listen("add", function () {
    div.innerHTML = count;
  });
})();
```

### 判断数据类型

```js
let isType = function (type) {
  return function (obj) {
    return Object.prototype.toString.call(obj) === `[object ${type}]`;
  };
};

let isString = isType("String");
let isArray = isType("Array");
let isNumber = isType("Numer");

console.log(333, isArray([1, 2, 3]));
```
