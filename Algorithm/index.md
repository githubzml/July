## 回文数

求用十进制，二进制，八进制表示都是回文数字的所有数字中，大于十进制数 10 的最小值。

```js
let num = 11;
String.prototype.reverse = function () {
  return this.split("").reverse().join("");
};

while (true) {
  if (
    num.toString() == num.toString().reverse() &&
    num.toString(8) == num.toString(8).reverse() &&
    num.toString(2) == num.toString(2).reverse()
  ) {
    console.log(num); // 585 1001001001 1111
    break;
  }
  num += 2;
}
```
