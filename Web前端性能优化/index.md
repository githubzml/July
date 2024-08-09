# Web 前端性能优化

## 压缩图片

1. [tinyPNG](https://tinypng.com/)

2. gulp 工具将图片基线式（重上往下）转化为渐进式（一遍遍渲染图片，从模糊到清晰）

但本人的 gulp-imagemin 一直有问题，终不能复现~

## 实现图片的延迟加载 Intersection Observer 方式

```vue
<template>
  <div class="box">
    <h1 style="color: black">图片由微博提供</h1>
    <div v-for="(item, index) in imgList" class="lazy">
      <img alt="图片" :data-src="item.url" />
    </div>
  </div>
</template>

<script setup>
import { onMounted } from "vue";

let imgList = [];

for (let i = 0; i < 30; i++) {
  imgList.push({
    url: `http://47.121.118.98:7003/${i + 1}.jpeg`,
  });
}

onMounted(() => {
  // 在 DOM 内容加载完毕后，执行延迟加载处理逻辑
  document.addEventListener("DOMContentLoaded", function () {
    let lazyImages = [].slice.call(document.querySelectorAll(".lazy img"));
    // 判断浏览器兼容
    if (
      "IntersectionObserver" in window &&
      "IntersectionObserverEntry" in window &&
      "intersectionRatio" in window.IntersectionObserverEntry.prototype
    ) {
      // 创建 IntersectionObserver 实例
      let lazyImageObserver = new IntersectionObserver(function (
        entries,
        observer
      ) {
        entries.forEach(function (entry) {
          // 判断是否进入可视区域
          if (entry.isIntersecting) {
            let lazyImage = entry.target;
            lazyImage.src = lazyImage.dataset.src;
            // 图片加载完成后，取消监听防止重复加载
            lazyImage.classList.remove("lazy");
            lazyImageObserver.unobserve(lazyImage);
          }
        });
      });
      lazyImages.forEach(function (lazyImage) {
        lazyImageObserver.observe(lazyImage);
      });
    }
  });
});
</script>

<style>
html,
body,
#app {
  height: 100%;
}

.lazy {
  width: 200px;
}

.lazy img {
  width: 100%;
}
</style>
```

## 原生的延迟加载支持 img 中的 loading="lazy" 属性

可能跟距离顶部距离有关 loading="lazy" ，本人在实验过程中也是有时可以，有时不行~

```vue
<img src="" loading="lazy" />
```

```vue
<template>
  <div>
    <h1 style="color: black">图片由微博提供</h1>
    <div style="height: 4000px"></div>
    <img
      src="https://gips3.baidu.com/it/u=45328832,131546734&fm=3039&app=3039&f=JPEG?w=1024&h=1024"
      loading="lazy"
      width="300px"
    />
    <div style="height: 4000px"></div>
    <img src="http://47.121.118.98:7003/1.jpeg" loading="lazy" width="200px" />

    <div style="height: 4000px"></div>
    <img src="http://47.121.118.98:7003/2.jpeg" loading="lazy" width="200px" />
  </div>
</template>

<style>
html,
body,
#app {
  height: 100%;
}

.lazy {
  width: 200px;
}

.lazy img {
  width: 100%;
}
</style>
```
