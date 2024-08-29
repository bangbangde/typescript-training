# [Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)

## Conditional Types - 条件类型

Conditional Types 用于根据条件类型的条件来获取类型，允许开发者基于某种条件来动态确定某个类型。

```ts
interface Animal {
  live(): void;
}
interface Dog extends Animal {
  woof(): void;
}

type Example1 = Dog extends Animal ? number : string; // number

type Example2 = RegExp extends Animal ? number : string; // string
```

Conditional types 的形式看起来有点像 JS 的条件表达式：
```ts
// 当 extends 左侧的类型可以赋值给 extends 右侧的类型时，返回 TrueType 类型，否则返回 FalseType 类型
SomeType extends OtherType ? TrueType : FalseType;
```

一般结合泛型使用：
```ts

```
