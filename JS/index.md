# Set 数据结构

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
