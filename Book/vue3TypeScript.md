### 类型推断

1. 定义变量时赋值，推断为对应类型
2. 定义变量时没有赋值 推断为 any 类型

```ts
// 定义变量时赋值，推断为对应类型
let val = 122; // number

val = "交通事故电话"; // error
```

```ts
/* 定义变量时没有赋值，推断为 any 类型*/
let anyType; // any 类型
anyType = 122;
anyType = "交通事故电话";
```

### 2.4.3 可选参数和默认参数

```ts
function getUrl(prefix: string = "/api/", url?: string): string {
  if (url) {
    return prefix + url;
  } else {
    return prefix;
  }
}

console.log(getUrl("/ctrl/", "base/getUserList")); // /ctrl/base/getUserList
console.log("/ctrl/"); // /ctrl/
console.log(getUrl()); // /api/
```

### 2.4.4 剩余参数

```ts
function getUrl2(prefix: string, ...urls: string[]) {
  return prefix + urls.join("/");
}
console.log(getUrl2("/base/", "user", "getList")); // /base/user/getList
```

### 2.5.1 泛型引入

```ts
function createArray(value: any, count: number): any[] {
  const arr: any[] = [];
  for (let index = 0; index < count; index++) {
    arr.push(value);
  }
  return arr;
}

const arr1 = createArray(17, 3);
const arr2 = createArray("额", 3);

console.log(arr1[0], arr2[0]);
```

### 2.5.2 使用泛型

```ts
function createArray2<T>(value: T, count: number) {
  const arr: Array<T> = [];
  for (let index = 0; index < count; index++) {
    arr.push(value);
  }
  return arr;
}
const arr3 = createArray2(17, 3);
const arr4 = createArray2("额", 3);

console.log(arr3[0].toFixed(), arr4[0].toFixed()); // 直接提示器报错
```

### 2.6.1 声明文件

很多第三方库都定义了对应的声明文件库，库文件名一般为 @types/xx 我们可以在 https://www.npmjs.com/ 上进行搜索

下载 react 声明文件的命令为： npm i @types/react

### 3.5.10 事件修饰符

.stop 阻止冒泡

```html
<div @click="handlerClick1">
  <el-button type="success" @click.stop="handlerClick2">Success</el-button>
</div>
```

```js
const handlerClick1 = () => {
  alert(1);
};
const handlerClick2 = () => {
  alert(2);
};
```

.prevent 阻止默认事件

```html
<div>
  <a href="http://baidu.com" @click.prevent="handlerClick3">baidu</a>
</div>
```

```js
const handlerClick3 = () => {
  console.log("handlerClick3");
};
```

.capture 添加事件监听器 使用事件捕获模式

```html
<div @click.capture="innerHandler4">
  <input type="button" value="按钮（capture）" @click="btnhandler" />
</div>
```

```js
/* .capture 可以优先捕获事件进行执行 */
const innerHandler4 = () => {
  console.log("innerHandler4");
};
```

.self 只有事件在该元素本身触发时才触发的回调

```html
<div @click.self="innerHandler5">
  <input type="button" value="按钮（self）" @click="btnhandler2" />
</div>
```

```js
/* .self 只给触发是自己的，绑定事件 */
const innerHandler5 = () => {
  console.log("5");
};
const btnhandler2 = () => {
  console.log("2");
};
```

.self 和 .stop 区别结论： .self 只会阻止自己身上冒泡行为 并不会真的阻止冒泡行为

### 5.4.2 Vue3 优先使用 Proxy

在使用 Vue2 的过程中 Vue 给 data 中的数组或者对象新增属性时，需要使用 vm.$set 才能保证新增的属性也是响应式的
