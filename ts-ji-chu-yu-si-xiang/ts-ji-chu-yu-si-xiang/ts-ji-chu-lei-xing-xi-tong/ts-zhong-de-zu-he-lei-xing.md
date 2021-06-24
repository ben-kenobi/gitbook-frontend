# TS中的组合类型



### **object:**

> 除了基本类型，object类型是TS开发中最常见的。任何带有属性字段的类型都是object类型。

看下object类型声明的三种基本方式：

```text
type Man = {               // 定义一个object类型 命名为Man
    name: string,
    gender: 'male'
};

interface Woman = {      // 使用interface方式定义一个object类型 命名为 Woman
    name: string,
    gender: 'female'
}

let man:Man;           // 使用Man类型约束变量man

let country:{countryName:string};    // 字面量方式声明一个object类型来约束变量country。

```

### \*\*\*\*

### **union:**

看下union类型语法：

```text
let strOrNum: string | number;  // strOrNum类型可以接收 string或number类型的数据。

type Man = {               // 定义一个Man类型
    name: string,
    gender: 'male'
};
type Woman = {            // 定义一个Woman类型
    name: string,
    gender: 'female'
};

type Country = { countryName: string };     //  定义一个Country类型

let manOrWoman: Man | Woman = {            // 声明一个类型是 Man或者Woman的 union类型
    name: 'han',
    gender: 'male'
};

let citizen: (Man | Woman) & Country = {    // 声明一个类型是Man或者Woman并合并Country类型  的union类型
    ...manOrWoman,
    countryName: 'cn'
};

```

> 可以通过 `｜` 或 `&` 将不同类型联结成一个union类型

\*\*\*\*

### **数组与元组：**

```text

let numAry:number[] = [1,2,3];
// 声明一个number数组

let numAry2:Array<number> = [1,2,3];
// 声明一个number数组，效果和number[]一样，一般使用number[]的形式。

let tuple:[number,string] = [1,'w'];
// 声明一个第一个类型是number，第二个类型是string的元组类型，元组就是一个定长数组。


```

\*\*\*\*

\*\*\*\*

### **函数：**

> TS里最上层的函数类型是Function，也可以声明更加具体的函数类型。

最基本的函数声明，这种声明仅仅约束了变量类型是个函数：

```text
// 约束了fn是一个函数
let fn: Function = function () { }
```

更加具体的函数声明，约束了参数类型，参数个数，返回值类型。

以下5种方式是等效的

```text
// 用不同方式为函数增加如下约束：有一个参数，参数类型是number，函数返回值类型是string

// 方式一
function fn1(a: number): string {
    return '';
}


// 方式二
let fn2: (a: number) => string = function (a) {
    return '';
}


// 方式三
type FnWithStrReturnType = (a: number) => string;

let fn3: FnWithStrReturnType = function (a) {
    return '';
}


// 方式四
type FnWithStrReturnType$2 = {
    (a: number): string;
}
let fn4: FnWithStrReturnType$2 = function (a) {
    return '';
}


// 方式五
interface FnWithStrReturnType$3 {
    (a: number): string;
}
let fn5: FnWithStrReturnType$3 = function (a) {
    return '';
}


// 第四第五两个方式都是声明了一个可调用的对象类型，因为函数在JS中也是对象
```

函数声明还支持泛型来约束不同**@变量** 之间的类型关系

```text
// 泛型 ，约束了 参数是一个数组，返回值是数组中的元素类型
function firstElement<Type>(arr: Type[]): Type {
    return arr[0];
}

let first = firstElement([2, 3, 4]);


```

可以将参数声明为可选，或给参数设置默认值：

```text
// 可选类型参数，一般需要对可选类型参数做非空判断后再使用
function optX(x?: number):void{
    if(x) {
        // ...
    } else {
    }
}

// 带默认值的参数
function defX(x = 10) :void{
    // ...
}
```

> JS中 `undefined`，`null`，`0`，`空字符串`，`NaN`，`false` 布尔运算后都是`false`。

Rest 形式参数声明：

```text
// rest形式参数以 ...name 方式声明，类型必须是数组类型。
function multiply(n: number, ...m: number[]): number[] {
    return m.map((x) => n * x);
}


// 调用方式1
const a = multiply(10, 1, 2, 3, 4);

// 调用方式2
const b = multiply(10, ...[1, 2, 3, 4]);
```

对象类型参数解构语法：

```text
type ABC = { a: number; b: number; c: number };

// 定义一个函数将abc类型对象中的所有值进行相加
function sum(abc: ABC) {
    return abc.a + abc.b + abc.c;
}


// 使用参数解构的方式，使用起来更方便
function sum2({ a, b, c }: ABC) {
  return a + b + c;
}


// 两种声明方式调用过程是一样的

let total = sum({ a: 1, b: 3, c: 9 });

let total2 = sum2({ a: 1, b: 3, c: 9 });
```

