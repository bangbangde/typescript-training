# [Indexed Access Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)

## Indexed Access Types - 索引访问类型

Indexed Access Types 用于获取对象类型中某个属性的类型：

```ts
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"]; // number
```

索引（上例的 `age`）本身是一个 __类型__ ，所以可以也使用 `unions`、`keyof` 等类型：

```ts
type I1 = Person["age" | "name"];
type I2 = Person[keyof Person];
type AliveOrName = "alive" | "name";
type I3 = Person[AliveOrName];
```

还可以使用 number 或 string 来获取相关对象属性的类型：
```ts
type A = {a:string, b:number, [index: number]: string | number};
type A1 = A[number];

type B = {a:string, b:number, [index: string]: string | number};
type B1 = B[string];
type B2 = B[number];
```
