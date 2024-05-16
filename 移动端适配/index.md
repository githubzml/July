# Vite 移动端适配解决方案

## afme-flexible 和 postcss-pxtorem

1.

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"
/>
```

2. npm i -S amfe-flexible

```sh
npm install postcss postcss-pxtorem --save-dev => yarn add postcss-pxtorem@5.1.1 防止 vant 报错

yarn add reset-css // 默认是 -S
```

3. main.ts 内

```ts
import "reset-css";

import "amfe-flexible";
```

4. vant 按需引入

```sh
# 通过 yarn 安装
yarn add vant

yarn add @vant/auto-import-resolver unplugin-vue-components unplugin-auto-import -D
```

### 在 Vite + Vue3 +Vant4 中按需引入组件，使用 Toast 提示信息不展示

原因： @vant/auto-import-resolver 这个插件对某些组件的样式无法引入

解决：取消@vant/auto-import-resolver 这个插件的样式依赖

```ts
export default defineConfig({
  plugins: [
    vue(),
    Components({
      resolvers: [
        VantResolver({
          importStyle: false,
        }),
      ],
    }),
  ],
});
```

main.ts

```ts
import "vant/lib/index.css";
```

## vw + rem 布局

设备为 100vw 设计稿为 1080px

0.09259259 vw = 1px

当根元素为 font-size: 0.09259259 vw

如果页面 banner 图宽度 1080px 则页面元素代码设置为 1080 rem

为了方便编码，手动 x 100;(以 100 为计算基准值)

则手动设置根元素 font-size：9.259259 vw ，页面元素代码设置为 10.8 rem(1080/100) <strong>这是推荐方法</strong>

总之就是 页面 元素大小 X 根元素大小 = 100 份

0.09259259\*1080 = 100

9.259259\*10.8 = 100

## font-size + px + rem 布局 相似于 font-size + vw + rem 布局

100 为假设值

设备根元素的大小 = 100 x（实际设备宽度/设计稿宽度）

页面元素设置值则为 = （设计稿宽度/100）rem
