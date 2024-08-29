# [Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

## `keyof` 类型操作符
`keyof` 操作符接受一个对象类型，并返回此对象的 __所有键的联合类型__ （该对象所有键的字符串字面量或数字字面量的联合类型）。

下例中, P 和 P2 是等价的：

```ts
type Point = { x: number; y: number };
type P1 = keyof Point;
type P2 = "x" | "y";
```

如果对象有__索引签名__, `keyof` 会返回索引签名的键的联合类型：

```ts
type Arrayish = { [n: number]: unknown };
type A1 = keyof Arrayish; // number
type A2 = number
 
type Mapish = { [k: string]: boolean };
type M1 = keyof Mapish; // string | number
type M2 = string | number;
```
M1 是 `string | number` 。这是因为 JavaScript 对象键总是被强制转换为字符串，
所以`obj[0]`总是与`obj["0"]`相同。
