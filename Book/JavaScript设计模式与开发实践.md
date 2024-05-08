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

待完善

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

## 13.3 职责链简易模式

```js
// 500 元订单
let order500 = function (orderType, pay, stock) {
  if (orderType === 1 && pay === true) {
    console.log("500元订金预购，得到100元优惠卷");
  } else {
    order200(orderType, pay, stock);
  }
};

// 200 元订单
let order200 = function (orderType, pay, stock) {
  if (orderType === 2 && pay === true) {
    console.log("200元订金预购，得到50元优惠卷");
  } else {
    orderNormal(orderType, pay, stock);
  }
};

// 普通订单
let orderNormal = function (orderType, pay, stock) {
  if (stock > 0) {
    console.log("普通购买，无优惠卷");
  } else {
    console.log("手机库存不足");
  }
};

order500(1, true, 500); // 500元订金预购，得到100元优惠卷
order500(1, false, 500); // 普通购买，无优惠卷
order500(2, true, 500); // 200元订金预购，得到50元优惠卷
order500(3, false, 500); // 普通购买，无优惠卷
order500(3, false, 0); // 手机库存不足
```

### 14.3.4 引入中介者模式

```html
<!-- 
    下拉选择框 colorSelect
    文本输入框 numberInput
    展示颜色信息 colorInfo
    展示购买数量信息 numberInfo
    决定下一步操作按钮 nextBtn
   -->

选择颜色：
<select id="colorSelect">
  <option value="">请选择</option>
  <option value="red">红色</option>
  <option value="blue">蓝色</option>
</select>

<div style="margin-top: 40px;"></div>

选择内存：
<select id="memorySelect">
  <option value="">请选择</option>
  <option value="32G">32G</option>
  <option value="16G">16G</option>
</select>

输入购买数量：<input type="text" id="numberInput" />
<div style="margin-top: 50px;"></div>
您选择了颜色：
<div id="colorInfo"></div>
<div style="margin-top: 40px;"></div>
您选择了内存：
<div id="memoryInfo"></div>
<div style="margin-top: 40px;"></div>
您输入了数量：
<div id="numberInfo"></div>
<div style="margin-top: 40px;"></div>
<button id="nextBtn" disabled="true">请选择手机颜色和购买数量</button>
```

```js
let colorSelect = document.getElementById("colorSelect"),
  memorySelect = document.getElementById("memorySelect"),
  numberInput = document.getElementById("numberInput"),
  colorInfo = document.getElementById("colorInfo"),
  memoryInfo = document.getElementById("memoryInfo"),
  numberInfo = document.getElementById("numberInfo"),
  nextBtn = document.getElementById("nextBtn");

let goods = {
  "red|32G": 3,
  "red|16G": 0,
  "blue|32G": 1,
  "blue|16G": 6,
};

let meditor = (function () {
  return {
    changed: function (obj) {
      let color = colorSelect.value,
        memory = memorySelect.value,
        stock = goods[color + "|" + memory],
        number = numberInput.value;

      if (obj === colorSelect) {
        colorInfo.innerHTML = color;
      } else if (obj === memorySelect) {
        memoryInfo.innerHTML = memory;
      } else if (obj === numberInput) {
        numberInfo.innerHTML = number;
      }

      if (!color) {
        nextBtn.disabled = true;
        nextBtn.innerHTML = "请选择手机颜色";
        return;
      }

      if (!memory) {
        nextBtn.disabled = true;
        nextBtn.innerHTML = "请选择内存大小";
        return;
      }

      if ((number - 0 || 0) !== number - 0) {
        nextBtn.disabled = true;
        nextBtn.innerHTML = "请输入正确购买数量";
        return;
      }

      if (number > stock) {
        nextBtn.disabled = true;
        nextBtn.innerHTML = "库存不足";
        return;
      }

      nextBtn.disabled = false;
      nextBtn.innerHTML = "放入购物车";
    },
  };
})();

colorSelect.onchange = function () {
  meditor.changed(this);
};
memorySelect.onchange = function () {
  meditor.changed(this);
};
numberInput.onchange = function () {
  meditor.changed(this);
};
```

## 15.4 装饰器模式

### 15.4.1 装饰函数 before

```html
<!-- 给对象动态增加职责的方式称为装饰器模式 decorator -->
<!-- 不改变函数源码的情况下，给函数增加功能 开放-封闭原则 -->
<button id="btn">点击</button>
```

```js
// // bed
// let a = function () {
//   alert(1)
// }
// // 改成
// let a = function () {
//   alert(1)
//   alert(2)
// }
// // good

// let a = function () {
//   alert(1)
// }

// let _a = a;
// a = function () {
//   _a();
//   alert(2)
// }

// 不确定这个事件是不是已经被其他人绑定过

window.onload = function () {
  alert(1);
};

let _onload = window.onload || function () {};

window.onload = function () {
  _onload();
  alert(2);
};

Function.prototype.before = function (beforeFn) {
  let _self = this;
  return function () {
    beforeFn.apply(this, arguments);
    return _self.apply(this, arguments);
  };
};

document.getElementById = document.getElementById.before(function () {
  console.log(1);
});

let btn = document.getElementById("btn");

console.log(2);
```

### 15.4.2 装饰函数 after

```js
Function.prototype.after = function (afterFn) {
  let _self = this;
  return function () {
    let ret = _self.apply(this, arguments);
    afterFn.apply(this, arguments);
    return ret;
  };
};

window.onload = (window.onload || function () {})
  .after(function () {
    console.log(2);
  })
  .after(function () {
    console.log(3);
  })
  .after(function () {
    console.log(4);
  });
```

### 15.4.3 装饰函数非污染原型

```js
let before = function (fn, beforeFn) {
  return function () {
    beforeFn.apply(this, arguments);
    return fn.apply(this, arguments);
  };
};

let a = before(
  function () {
    alert(3);
  },
  function () {
    alert(2);
  }
);
a = before(a, function () {
  alert(1);
});
a();
```

### 15.4.4 数据统计上报 Demo

```html
<button id="login">点击打开登录浮层</button>
```

```js
// not-good
// let showLogin = function () {
//   console.log("打开登录浮层");

//   log()
// }

// let log = function () {
//   console.log("上报标签为")
// }

// document.getElementById("login").onclick = showLogin

// 装饰器模式 good

Function.prototype.after = function (afterFn) {
  let _self = this;
  return function () {
    let ret = _self.apply(this, arguments);
    afterFn.apply(this, arguments);
    return ret;
  };
};

let showLogin = function () {
  console.log("打开登录浮层");
};

document.getElementById("login").onclick = showLogin.after(function () {
  console.log("上报标签为");
});
```

## 判断数据类型

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
