# [Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)

## `typeof` 类型操作符

注意区别于 js 中的 `typeof` 操作符

ts 中的 `typeof` 操作符用于类型系统，在类型上下文中使用它来引用变量或属性的类型。

```ts
let s = "hello";
type S = typeof s;
```

> 注意 ts 中的 `typeof` 的操作数是一个变量（不能是字面量值），而不是一个类型，其返回值是一个类型。


## limitations
typeof 的操作数必须是 identifiers (变量名或变量属性标识符)。

也就是说 ts 的 typeof 只能用于获取类型信息，不会执行表达式。

```ts
let shouldContinue: typeof msgbox("Are you sure you want to continue?"); // Error
```
