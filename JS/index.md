## Set 数据结构

Set 没有重复的值

```js
const set = new Set([1, 2, 3, 4, 4, 5, 5, 6]);

console.log([...set]); // set 结构不会添加重复的值 [1,2,3,4,5,6]

console.log(set.size); // 成员个数 6
```

例子

去除字符串里面的重复字符

```js
console.log([...new Set("abcabcbcnm")].join("")); // abcnm
```

方法

```js
Set.prototype.add(value); // 添加某个值 返回 Set 结构本身
Set.prototype.delete(value); // 删除某个值，返回布尔值，表示删除是否成功
Set.prototype.has(value); // 返回布尔值表示该值是否为 set 的成员
Set.prototype.clear(); // 清除所有成员 没有返回值
```

对象的写法

```js
const properties = {
  weight: 1,
  height: 1,
};
if (properties[someName]) {
  // do something
}
```

Set 写法

```js
const properties2 = new Set();
properties2.add("weight");
properties2.add("height");

if (properties2.has(someName)) {
  // do something
}
```

## 原型相关

Object.hasOwn()

如果指定的对象自身有指定的属性，则静态方法 Object.hasOwn() 返回 true。如果属性是继承的或者不存在，该方法返回 false。

```js
Object.hasOwn(obj, prop);
```

:exclamation: 备注 ： Object.hasOwn() 旨在取代 Object.prototype.hasOwnProperty()。

### 直接属性和继承属性

以下示例区分了直接属性和通过原型链继承的属性：

```js
const example = {};
example.prop = "exists";

// `hasOwn` 静态方法只会对目标对象的直接属性返回 true：
Object.hasOwn(example, "prop"); // 返回 true
Object.hasOwn(example, "toString"); // 返回 false
Object.hasOwn(example, "hasOwnProperty"); // 返回 false

// `in` 运算符对目标对象的直接属性或继承属性均会返回 true：
"prop" in example; // 返回 true
"toString" in example; // 返回 true
"hasOwnProperty" in example; // 返回 true
```

## 垃圾回收机制

引用计数 （循环引用存在问题）

标记-清除算法 （2012 年后所有现代浏览器采用的算法--循环引用不再是问题）

---

## 闭包的性能考量

如果不是某些特定任务需要使用闭包，在其他函数内创建函数是不明智的，因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响。
