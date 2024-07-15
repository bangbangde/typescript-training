# TypeScript Handbook 阅读笔记

## [The TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

### 关于本手册
阐述JavaScript的发展和复杂性，TypeScript作为静态类型检查工具的目的。

### 手册结构

手册分为两部分

- 手册：提供TypeScript的全面指南，帮助读者理解常用语法和编译器选项。
- 参考文档：深入解释TypeScript的特定概念和行为。

### 非目标（Non-Goals）

手册不全面介绍JavaScript基础，不替代语言规范，不涉及工具配置。

### 入门
提供不同编程背景的入门页面建议。



## [The Basics](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)

开篇举例介绍了 JavaScript 的动态类型问题。由此引出静态类型检查系统。

### Static type-checking

强调了静态类型检查在提前发现和预防代码错误中的作用，以及 TypeScript 作为一个静态类型检查器如何帮助开发者编写更可靠和更高质量的代码。

> 静态类型系统描述了我们程序运行时值的形状和行为（shapes and behaviors）。
>
> TypeScript之类的类型检查器会利用这些信息，当出现问题时及时提醒我们。

### Non-exception Failures

此小节讨论了静态类型系统如何处理那些 __在运行时不会抛异常__ 的操作。比如 __访问对象上不存在的属性__ 。

虽然这可能会降低代码的表达能力，但可以避免很多错误。比如：

- 拼写错误
- 调用函数时忘记写括号
- 基本的逻辑错误


> a static type system has to make the call over what code should be flagged as an error in its system, even if it’s “valid” JavaScript that won’t immediately throw an error.

### Emitting with Errors

介绍了使用 `--noEmitOnError` 的场景

### Explicit Types（显式类型）⭐

介绍了如何 __add type annotations__

```ts
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
```

### Strictness
此小节强调了在TypeScript中使用严格模式的重要性，以及如何通过调整严格性设置来提高代码的类型安全性和减少潜在的错误。

两个最重要的标志是`noImplicitAny`和`strictNullChecks`。

- 打开 noImplicitAny 标志会对任何类型被隐式推断为 any 的变量发出错误提示。
- 启用 strictNullChecks 后, null 和 undefined 各自拥有类型，不能再赋给其他类型变量。

## [Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

介绍了常用的类型以及如何让给变量、函数添加类型注解。

### contextual typing

contextual typing 是指在上下文中自动推断类型。如匿名函数的参数类型推断：
```ts
const names = ["Alice", "Bob", "Eve"];

// Contextual typing for function
names.forEach(function (s) {
  console.log(s.toUpperCase());
});
```

### TypeScript 支持的类型

- 原始类型：string, number, boolean, bigint, symbol, null, undefined
- 其它类型：object, array, tuple, enum, any, unknown, never


### Union Types
>  使用操作符和已有类型构造新的类型

联合类型是一种由两个或多个其他类型组成的类型(使用 `|`分隔)，表示可以是这些类型之一的值。我们将这些类型中的每一个称为联合类型的成员。

```
let id: number | string;
```

使用联合类型时，通常需要使用类型守卫（Type Guards, 如 typeof、Array.isArray 等）来在代码中缩小联合类型的范围（narrow the union with code），TypeScript 会对此进行检查避免潜在的类型错误。

```ts
function printId(id: number | string) {
  if (typeof id === "string") {
    // In this branch, id is of type 'string'
    console.log(id.toUpperCase());
  } else {
    // Here, id is of type 'number'
    console.log(id);
  }
}
```

### Type Aliases
> 给 __任意类型__ 起一个别名，方便复用。

```ts
type ID = number | string;

type Point = {
  x: number;
  y: number;
};
```

### Interfaces
给 __对象类型__ 命名的另一种方式

```ts
interface Point {
  x: number;
  y: number;
}
```

### Type Aliases vs Interfaces
扩展性
- Type Aliases 定义后不可修改，只能通过 & 生成新的别名
- Interfaces 可以扩展 （extends、merge)，编译性能更好

原始类型
- Type Aliases 可以是原始类型
- Interfaces 只能是对象类型

### Type Assertions
类型断言
使用场景如下：有时，您会获得 TypeScript 无法了解的值类型信息。

使用 as 语法
```ts
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

使用尖括号语法（在tsx 中不可用）
```ts
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
``` 

### Literal Types
字面量类型

约束变量或参数为特定的值，而不是某种通用的类型。

通常与联合类型一起使用，以创建更复杂的类型约束。

```ts
let EventName: "click" | "scroll" | "mousemove";

let obj: { counter: 0 };
```

#### 字面量推断
Literal Inference（字面量推断）在 TypeScript 中指的是编译器如何推断字面量类型。

它是 TypeScript 类型系统中的一个特性，当你在定义变量或常量时，编译器会根据你赋予的字面量值来推断最具体的类型，而不是更通用的类型。

下面是使用 `as const` 将整个对象转换为字面量类型的例子：
```ts
const req = { url: "https://example.com", method: "GET" } as const;
```

### null and undefined
strictNullChecks 开启时，null 和 undefined 各自拥有类型，不能再赋给其他类型变量。且要使用缩小范围来分别检查值是否为 null 或 undefined。

#### 非空断言
Non-null Assertion Operator (Postfix `!`)
```ts
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

### Enums
向运行时添加的特性，非必要不使用。

### 不太常用的原始类型
bigint、symbol

## [Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

TypeScript 会追踪程序可能的执行路径，以分析某个位置上值的最具体可能类型。它会查看这些特殊检查（称为类型保护）和赋值操作，并将类型细化为比声明时更具体的类型，这个过程称为 __‘类型收窄’__。

### 类型保护（type guard）
> Within our if check, TypeScript sees `typeof padding === "number"` and understands that as a special form of code called a __type guard__. 

### 类型谓词 Using type predicates

type predicates（类型谓词）是一种自定义 __类型保护__ 的方式。

具体做法就是定义一个返回类型是 type predicate 的 函数。

看下例，`pet is Fish` 就是一个 type predicate。

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}
```

> 一个 predicate 的形式就是：`parameterName is Type`，其中 parameterName 必须是当前函数签名中的参数名称。


### Assertion functions

类型层面的 __断言函数__。

例如：

```ts
// 断言 condition 是 truthy
function assert(condition: any, msg?: string): asserts condition {
  if (!condition) {
    throw new AssertionError(msg);
  }
}

// 断言 val 是 string 用了 类型谓词
function assertIsString(val: any): asserts val is string {
  if (typeof val !== "string") {
    throw new AssertionError("Not a string!");
  }
}
```

### Discriminated unions

这是一种特殊的联合类型（已经被区分的联合类型）。

> When every type in a union contains a common property with literal types, TypeScript considers that to be a __discriminated union__, and can narrow out the members of the union.

联合类型的成员类型含有字面量类型的共有属性（区分符），这个区分符使得不同的成员类型可以被明确地区分和处理。

下例中，Shape 就是一个 __discriminated union__ 类型，kind 属性是区分符。

```ts
interface Circle {
  kind: "circle";
  radius: number;
}
 
interface Square {
  kind: "square";
  sideLength: number;
}
 
type Shape = Circle | Square;
```

### never & Exhaustiveness checking

收窄类型时，在类型选项减少到消除所有可能性并且没有任何剩余的位置时，TypeScript 将使用 never 类型来表示不应该存在的状态。

详尽性检查：收窄类型至 never。

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

## [More on Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html)
TS 有许多方式描述函数的调用方式。

### Function Type Expressions
最简单的方式 - 函数类型表达式。

这种类型的语法很像箭头函数：`(a: string) => void`。

```ts
function greeter(fn: (a: string) => void) {
  fn("Hello, World");
}
 
function printToConsole(s: string) {
  console.log(s);
}
 
greeter(printToConsole);
```

### Call Signatures
js中函数是一种特殊的对象，也是可以有属性的，描述这种函数可以使用 - __调用签名__。

```ts
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};
```

> 注意和 函数类型表达式 的细微区别：
> 
> `(a: string) => void`
>
> `(someArg: number): boolean`

### Construct Signatures
为构造函数准备的 - 改造 call signatures ( 前面加个 new )。

```ts
type SomeConstructor = {
  new (s: string): SomeObject;
};
```

有的函数既可以直接调用，也可以作为构造函数使用：
```ts
interface CallOrConstruct {
  (n?: number): string;
  new (s: string): Date;
}
```

### Generic Functions
函数的 返回值类型和参数类型相关连 或 多个参数的类型相关连 - 范型函数。

TS 中使用 __范型（generics）__ 来描述两个值是一致的，
做法是在函数签名中声明 __类型参数（type parameter）__ 来做到这一点：

```ts
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}
```

上例中的 `Type` 就是一个类型参数。

### Inference

TypeScript 会根据函数调用的上下文来 __推断__ 出 __类型参数__ 的值。

```ts
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}
 
// Parameter 'n' is of type 'string'
// 'parsed' is of type 'number[]'
const parsed = map(["1", "2", "3"], (n) => parseInt(n));
```

### Constraints

约束