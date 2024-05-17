# 大屏适配基础版

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <img src="./images/img.png" alt="" />
    </div>
    <script type="module">
      import { autoScale } from "./js/index.js";
      autoScale("#app", {
        width: 1920,
        height: 1080,
      });
      // let app = document.querySelector("#app");
      // console.log(123, app);
      // console.log(234, app.width);
    </script>
  </body>
</html>
```

```js
function throttle(fn, wait = 200) {
  let timer = null,
    isFirst = true;
  return function () {
    if (isFirst) {
      fn.apply(this, arguments);
      return (isFirst = false);
    }
    if (timer) {
      return;
    }
    timer = setTimeout(() => {
      fn.apply(this, arguments);
      timer = null;
      clearTimeout(timer);
    }, wait);
  };
}
export function autoScale(selector, options) {
  const el = document.querySelector(selector);
  const { width, height } = options;

  el.style.transformOrigin = "top left"; // 设置元素变形原点
  el.style.transition = "transform .5s";
  function init() {
    const scaleX = innerWidth / width;
    const scaleY = innerHeight / height;
    const scale = Math.min(scaleX, scaleY); // 判断以哪个缩放为准

    const left = (innerWidth - width * scale) / 2; // 让大屏始终保持居中展示
    const top = (innerHeight - height * scale) / 2;
    el.style.transform = `translate(${left}px, ${top}px) scale(${scale})`;
  }
  init();
  addEventListener("resize", throttle(init));
}
```

可以直接使用 v-scale-screen 插件
