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

## 数列的四则运算

例如

1234 -> 1 + 2\*3 - 4 = 3

9876 -> 9 \* 87 + 6 = 789

条件：假设 “将原来数字各个数位上的数逆序排列得到的数” 并且算式的运算按照四则运算的顺序进行（先乘除，后加减）
那么位于 100 -> 999 符合条件的有以下几种情况

351 -> 3 \* 51 = 153

621 -> 6 \* 21 = 126

886 -> 8 \* 86 = 688

问题：
求位于 1000 ~ 9999，满足上述条件的数

补充 1;

```js
// eval 执行表达式函数
let x = 4;
let y = 3;
console.log(eval("x + y")); // 7
console.log(eval("x - y")); // 1
console.log(eval("x * y")); // 12
console.log(eval("x / y")); // 1.33333...
```

补充 2;

```js
// charAt 一种字符串的方法 用于检索字符串中特定位置的字符
let a = "abc";
console.log(a.charAt(1)); // 下标为_1_的字符为 b
```

解题 3;

```js
// let flag = ["+", "-", "*", "/",""]
let flag = ["*", ""];
let func = () => {
  for (let i = 1000; i < 10000; i++) {
    let c = String(i);
    for (let j = 0; j < flag.length; j++) {
      for (let k = 0; k < flag.length; k++) {
        for (let l = 0; l < flag.length; l++) {
          let val =
            c.charAt(3) +
            flag[j] +
            c.charAt(2) +
            flag[k] +
            c.charAt(1) +
            flag[l] +
            c.charAt(0);
          if (val.length > 4) {
            if (i == eval(val)) {
              console.log(val + "=" + i);
            }
          }
        }
      }
    }
  }
};
func(); // => 5*9*31=1395
```
