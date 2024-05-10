# return 和 return false 区别

相同点 ： 后面代码均不执行

```js
let m = (function () {
  console.log(222); // 执行
  return;
  console.log(333); // 不执行
})();

let n = (function () {
  console.log(555); // 执行
  return false;
  console.log(666); // 不执行
})();
```

不同点：值类型不同

return 返回的是 undefined

return 返回的是 false

```js
function treturn() {
  return false;
}
function nreturn() {
  return;
}
if (nreturn() === false) alert("Yes");
else alert("No");
```
