# 联合类型 （可能包含有某一个类型）

1. 基础联合类型

```ts
let data: string | number;
data = "hello ts";
data = 123;
data = false; // 编译错误：不能将类型“boolean”分配给类型“string | number”。ts(2322)
```

2. 对象联合类型

```ts
interface Admin {
  name: string;
  age: number;
}
interface User {
  name: string;
  sayHi(): void;
}
declare function Employee(): Admin | User;
let employee = Employee();
employee.name = "Echo";
// 下面语句会报错：age属性不是 Admin 和 User 共有的属性
employee.age = 26; // 编译错误：类型“Admin | User”上不存在属性“age”。类型“User”上不存在属性“age”。ts(2339)
```

上面这段代码中，定义了两个接口 Admin 和 User，接着使用 declare function 声明了一个 Employee 函数，该函数的返回类型为 Admin 或 User。之后通过调用 Employee 函数并将返回值赋给了 employee 变量。接着将 employee 对象中 name 属性的值设置为 'Echo' 是可以的，因为 name 属性是 Admin 和 User 共有的属性。而将 employee 对象中 age 属性的值设置为 26 时会出现编译错误，错误信息指出类型 Admin | User 上不存在属性 age。这是因为 age 属性只存在于 Admin 接口中，而不属于 User 接口。

造成该错误的原因是，**TypeScript 在联合类型上只能访问联合类型中所有类型的共有属性和方法。** 因此，通过联合类型的变量只能访问 name 属性，而不能访问 age 属性。

3. 字面量联合类型

```ts
let direction: "Up" | "Right" | "Down" | "Left";
direction = "Right";
direction = "none"; // 编译错误，只能取值为 "Up" | "Right" | "Down" | "Left"
```

# 交叉类型 （必须包含）

```ts
type Person = {
  name: string;
};
type User = {
  age: number;
};
let person: Person & User;
person = {
  name: "Echo",
  age: 26,
};
// 编译错误：
// 不能将类型“{ name: string; }”分配给类型“Person & User”。
// 类型 "{ name: string; }" 中缺少属性 "age"，但类型 "User" 中需要该属性。ts(2322)
// index.ts(7, 3): 在此处声明了 "age"。
person = {
  name: "Steven",
};
```
