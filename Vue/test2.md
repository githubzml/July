# vue

### slot 示例 1

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

### 组件

```vue
<BaseLayout>
  <template v-slot:header>
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```

### const 和 Vue3 readonly 区别

都可作为只读代理

### watch() 与 watchEffect（）区别

watch() 默认是懒监听 即仅在侦听源发生变化时才执行回调函数

watch() 第一个参数是侦听器的源 这个来源可以使以下几种：

- 一个函数，返回值

- 一个 ref

- 一个响应式对象

watch() 第二个参数是发生变化时要调用的回调函数。回调函数接受三个参数 新值 旧值 以及一个用于注册副作用清理的回调函数。

watch() 第三个可选参数是一个对象。 支持一下这些选项

1. immediate： 在侦听器创建时立即触发回调。第一次调用的旧值是 undefined

2. deep： 如果源是对象，强制深度遍历 以便在深层级变更时触发回调。

---

与 watchEffect() 相比 watch() 使我们可以

- 懒执行副作用
- 更加明确应该是由那个状态触发侦听器重新执行
- 可以访问侦听状态的前一个值和当前值

* watchEffect() 示例

```js
const count = ref(0);

watchEffect(() => console.log(count.value));
// -> 输出 0

count.value++;
// -> 输出 1
```

watchEffect() 手动停止侦听器：

```js
const stop = watchEffect(() => {});

// 当不再需要此侦听器时:
stop();
```

watch() 手动停止侦听器：

```js
const stop = watch(source, callback);

// 当已不再需要该侦听器时：
stop();
```

停止侦听器

在 setup() 中用同步语句创建的侦听器，会自动绑定到宿主组件实例上，并且会在宿主组件卸载时自动停止。<strong>因此，在大多数情况下，你无需关心怎么停止一个侦听器</strong>。

### computed()

接受一个 getter 函数，返回一个只读的响应式 ref 对象。 该 ref 通过 .value 暴露 getter 函数 的返回值。它也可以接受一个带有 get 和 set 函数的对象来创建一个可写的 ref 对象。

创建一个可写的计算属性 ref：

```js
const count = ref(1);
const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1;
  },
});

plusOne.value = 1;
console.log(count.value); // 0
```

### nextTick()

https://blog.csdn.net/omroji/article/details/136064888

等待下一次 DOM 更新后调用的方法

Vue 在修改数据后 视图不会立刻更新 而是等同一事件循环中的所有数据变化完成之后 再同一进行视图更新

nextTick() 方法就是在这样的 DOM 更新循环结束后调用指定函数

简单理解就是 当数据更新了 在 DOM 中 渲染后 ，自动执行函数

应用场景

与第三方库集成 确保动画 或者 绘图 在正确的元素上运行

nextTick() 可以帮助你确保 DOM 已经更新并准备好与第三方库集成。

---

npm 包升级问题

节流 去抖动问题

设计模式

https://juejin.cn/post/7145365108275806239

---

Vite 中 Typescript + SWC 和 不加 SWC 区别

https://cn.vitejs.dev/guide/performance.html#use-lesser-or-native-tooling

官网原话 减少源文件（JS/TS/CSS）的工作量
通俗理解 swc 加快编译速度

参考链接：
https://view.inews.qq.com/qd/20230118A042YJ00

全局组件引入 注册

全局组件有以下几个问题：

1. 全局组件即使它没有被实际使用，它仍然会出现在打包后的 JS 文件中（不会被 “tree-sharking”）
2. 在父组件中使用子组件 不太容易定位子组件的实现 和过多使用的全局变量一样 可能会影响应用长期的可维护性

### vue3 使用 element-plus 动态渲染 Icon 图标

```code
<el-icon><component :is="item.icon" /></el-icon>
```

### v-model 可以在组件上使用以实现双向绑定 defineModel() 宏

v-model 可以在组件上使用以实现双向绑定。

从 Vue 3.4 开始，推荐的实现方式是使用 defineModel() 宏：

```vue
<!-- Child.vue -->
<script setup>
const model = defineModel();

function update() {
  model.value++;
}
</script>

<template>
  <div>parent bound v-model is: {{ model }}</div>
</template>
```

父组件可以用 v-model 绑定一个值：

```vue
<!-- Parent.vue -->
<Child v-model="countModel" />
```

在 3.4 版本之前，你一般会按照如下的方式来实现上述相同的子组件：

```vue
<script setup>
const props = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue"]);
</script>

<template>
  <input
    :value="props.modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  />
</template>
```
