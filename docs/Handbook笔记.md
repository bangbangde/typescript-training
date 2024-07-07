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



## [基础（The Basics）](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)

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
type Point = {
  x: number;
  y: number;
};
```