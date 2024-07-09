## 类型注解写法

变量类型注解
```ts
let myName = "Frank"; // 通常不用写，ts会自动推断:)
let age: number = 18;
```

函数参数类型注解，可以使用 ? 标记为可选（所有必选参数必须放在可选参数之前）
```ts
function greet(name: string, age?: number) {
  console.log("Hello, " + name.toUpperCase() + "!!");
}
```

函数返回类型注解
```ts
function getFavoriteNumber(): number {
  return 26;
}
```

## TypeScript 中的类型

- JS 原始类型： string, number, boolean, bigint, symbol, null, undefined
- 特殊类型：void, never, any, unknown
- 复杂类型：object, array, tuple, enum
- 复合类型：union types, intersection types, type alias, interface

对象类型
```ts
let val9: object;

// 对象字面量
let val10: { name: string, age: number };
let val11: { name: string, age?: number }; // 可选属性
let val12: { name: string, [key: string]: any }; // 任意属性
```

数组
```ts
let val13: Array<number> = [1, 2, 3];
let val14: number[] = [1, 2, 3];
```

Union Type
```ts
let val15: string | number = "hello";
```

Type Aliases -- 给任意类型起一个别名，方便复用。
```ts
type Point = {
  x: number;
  y: number;
};

let val16: Point = { x: 1, y: 2 };
```

Interfaces, 给 对象类型命名的另一种方式
```ts

```

## void 的使用场景

函数返回类型，表示没有返回值
```ts
function logMessage(message: string): void {
  console.log(message);
}
```
void 类型的变量只能被赋值为 undefined 或 null（在严格模式下，仅 undefined）

没有实际使用场景也没有意义

## never 的使用场景

永远抛出错误的函数
```ts
function throwError(message: string): never {
  throw new Error(message);
}
```
进入无限循环的函数
```ts
function infiniteLoop(): never {
  while (true) {}
}
```

不可能存在的类型

帮助我们确保所有可能的情况都得到了处理

```ts
type Foo = string | number;

function checkType(value: Foo) {
    if (typeof value === "string") {
        console.log("It's a string");
    } else if (typeof value === "number") {
        console.log("It's a number");
    } else {
        // 这里的 value 类型会被推断为 never
        // 如果有新的类型被添加到 Foo，TS会 报告错误
        const exhaustiveCheck: never = value;
        console.log(exhaustiveCheck);
    }
}
```

## JavaScript 的数据类型：

原始类型 (Primitive values)
|Type|`typeof` return value|Object wrapper|
|--|--|--|
|String          |"string"   |String |
|Number          |"number"   |Number |
|Boolean         |"boolean"  |Boolean|
|BigInt          |"bigint"   |BigInt |
|Symbol          |"symbol"   |Symbol |
|Null            |"object"   |N/A    |
|Undefined       |"undefined"|N/A    |

对象（Objects）

|Type|`typeof` return value|
|--|--|
|Function        |"function" |
|Any other object|"object"   |


> 在 ECMA-262 规范中，实现了 [[Call]] 内部方法的对象被视为函数
>
> ES6的类只是个特殊的函数