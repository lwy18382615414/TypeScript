TypeScropt

## 一、面向JavaScript的TypeScript

1、推断类型

```ts
let helloWorld = "Hello World";
	let helloWorld: string;
```

2、定义类型

```ts
//用interface声明此对象的形状
interface User {
    name: string;
    idL number;
}
//创建一个包含name: string和id: number的推断类型的对象
const user: User = {
	name: "joe",
	id: 0;
}
//若对象与接口不匹配则会报错
```

3、组合类型

```ts
//基本用法
type MyBool = true | false;

//联合类型也可以是string 或 number 字面的集合
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;

//联合也提供处理不同类型的方法
function getLength(obj: string | string[]) {
  return obj.length;
}

//补充typeof用法
字符串	typeof s === "string"
数字	typeof n === "number"
布尔值	typeof b === "boolean"
undefined	typeof undefined === "undefined"
函数	typeof f === "function"
数组	Array.isArray(a)
```

4、泛型： 泛型为类型提供变量。

泛型是指当定义函数/接口类型时，不预先指定具体的类型，而是在使用时指定类型限制的一种特性

```ts
//函数在使用泛型
function test <T>（相等于定义形参） (arg:T):T{
  console.log(arg);
  return arg;
}
test<number>(111);// 返回值是number类型的 111
test<string | boolean>('hahaha')//返回值是string类型的 hahaha
test<string | boolean>(true);//返回值是布尔类型的 true

//接口中使用泛型
interface Search {
  <T,Y>(name:T,age:Y):T
}

let fn:Search = function <T, Y>(name: T, id:Y):T {
  console.log(name, id)
  return name;
}
fn('li',11);//编译器会自动识别传入的参数，将传入的参数的类型认为是泛型指定的类型
```

## 二、TypeScript日常类型

1、原语：string、number和boolean

2、数组：number[]、string[]亦或是Array<number> 

3、元组Tuple



4、变量的类型注释

```ts
let myName: string = "joe"
```

5、函数

```ts
//参数类型的注解
//声明函数是，可以在函数的每个参数后面加上注解，声明函数接受哪些类型的参数
function greet(name: string) {
  console.log("Hello, " + name.toUpperCase() + "!!");
}
//返回类型注解
function getFavoriteNumber(): number {
  return 26;
}
```

6、对象类型

```ts
//可选属性
//对象类型可以指定其部分或全部属性是否为可选。可选的属性名称后面加上?
function printName(obj: { first: string; last?: string }) {}
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
//在JavaScript中，如果访问对象一个不存在的属性会获得undefined值。所以在访问一个可能不存在的值时需检查
function printName(obj: { first: string; last?: string }) {
    console.log(obj.last); //obj.last' is possibly 'undefined
    if(obj.last){
        console.log(obj.last);
    }
}
```

7、联合类型

```ts
//在使用联合类型时，若 string | number ，则不能使用仅在string上可用的方法
function printId(id: number | string) {
  console.log(id.toUpperCase());
Property 'toUpperCase' does not exist on type 'string | number'.
  Property 'toUpperCase' does not exist on type 'number'.
}
//要正确使用要加上对值的判断
function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}
```

8、类型别名

```ts
//当希望多次使用同一个类型并用一个名称引用时，类型别名就非常重要
type Point = {
  x: number;
  y: number;
};
```

9、接口

```ts
//接口声明时命名对象类型的另一种方式
interface Point {
  x: number;
  y: number;
}
//类型别名与接口的区别
//两者非常相似，很多情况下都能自由选择；主要区别在于类型别名无法重新打开类型添加新的属性，而接口始终可以拓展

//接口的拓展
interface Animal {
  name: string;
}

interface Bear extends Animal {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;

//通过交际拓展类型
type Animal = {
  name: string;
}

type Bear = Animal & { 
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;

//向现有接口添加新的字段
interface Window {
  title: string;
}

interface Window {
  ts: TypeScriptAPI;
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});

//类型创建后无法更改
type Window = {
  title: string;
}

type Window = {
  ts: TypeScriptAPI;
}

 // Error: Duplicate identifier 'Window'.
```

10、类型断言

```ts
//当获得一个TypeScript无法知道的类型信息时，使用类型断言

//使用 document.getElementById，TypeScript 只知道这将返回某种 HTMLElement，但页面将始终要给定 ID 的 HTMLCanvasElement。

//在这种情况下，可以使用类型断言来指定更具体的类型：

const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

11、非空断言运算符（后缀!）

```ts
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

## 三、类型运算

1、条件  extends ? :

```typescript
type isTwo<T> = T extends 2 ? true : false;
type res = isTwo<1>;
```

2、约束  extends

```typescript
// 通过T extends Length 约束了T的类型，必须是包含length属性，且length的类型必须是number
interface Length {
	length： number
}

function fn1<T extends Length>(arg: T): number {
	return arg.length
}
```

3、推导 infer

推导类似js 的正则匹配，都满足公式条件时，可以提取公式中的变量，直接返回或者再次加工都可以

```typescript
// 推导：infer
// 提取元组类型的第一个元素：
// extends 约束类型参数只能是数组类型，因为不知道数组元素的具体类型，所以用 unknown。
// extends 判断类型参数 T 是不是 [infer F, ...infer R] 的子类型，如果是就返回 F 变量，如果不是就不返回
type First<T extends unknown[]> = T extends [infer F, ...infer R] ? F : never;
type res2 = First<[1, 2, 3]>; // 1
```

4、联合 |

```ts
type Union = 1 | 2 | 3
```

5、交叉 &

```ts
//交叉代表对类型做合并
type ObjType = { a: number } & { c: boolean }
```

6、类型映射 （对索引类型作修改）

```ts
// 类型映射就相当于把一个集合映射到另一个集合
type MapType<T> = {
	[Key in keyof T]? : T[key]
}
// keyof T 是查询索引类型中所有的索引，叫做索引查询
// T[Key] 是取索引类型某个索引的值，叫做索引访问
// in 是用于遍历联合类型的运算符
 
 // 映射 改变值
type MapType<T> = {
	[Key in keyof T] : [T[key], T[Key], T[key]]
}
type res = MapType<{a: 1, b: 2}>
type res = {
    a: [1, 1, 1];
    b: [2, 2, 2]
}

存在一个类型 Todo，自定义一个类型 MyReadonly 可以将 Todo 类型里面的属性都转换成 readonly
interface Todo {
  title: string
  description: string
}
type MyReadonly<T> = { readonly [K in keyof T]: T[K] }
type ReadonlyTodo = MyReadonly<Todo> 
/*
 * 输出
 * ReadonlyTodo {
 *   readonly title: string
 *   readonly description: string
 * }
 **/
```

