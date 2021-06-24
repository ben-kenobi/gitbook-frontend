# TS中类型的声明方式



### **literal方式：**

> TS类型系统是 结构化类型系统，某个数据类型 就是指这个数据的结构，因此我们可以直接使用一个数据结构来表示一个类型。这种声明类型的方式就是literal方式。

```text
let a:{name:string} = {name:'han'};  // literal方式声明 {name:string} 这种结构的类型


// literral 方式声明 (name:string)=>string 这种结构的函数
let greet:(name:string)=>string = function(name){
    return `hello: ${name}`;
}
```

### **type alias方式：**

> 除了literal 方式声明类型，还可以通过给一种结构类型 取一个别名的方式，方便多处使用该结构类型。

```text

// type alias 方式给 {name:string} 这种结构的类型 取一个别名User
type User = {
  name: string;
}

let a: User = { name: 'han' };

// type alias 方式给 (name:string)=>string  这种结构的函数 取一个别名Fn
type Fn = (arg: string) => string;


let greet: Fn = function (name) {
  return `hello: ${name}`;
}
```

### **interface方式：**

> Interface 方式也是一种给类型取别名的方式

```text
// interface方式给 {name:string} 这种结构的类型 取一个别名User
interface User {
  name: string;
}

interface ExUser extends User {
  nickName: string;
}

let a: ExUser = { name: 'yf', nickName: 'han' };

// interface 方式给 (name:string)=>string  这种结构的函数 取一个别名Fn,注意在interface方式中 ，=> 需要改成 : 
interface Fn {
  (arg: string): string;
}


let greet: Fn = function (name) {
  return `hello: ${name}`;
}


// class 可以 implements 某个类型,implements 某个类型后必须实现类型中所有的属性。
class AA implements User {
  name: string;
}
```

**interface方式与type alias方式的区别：**

1. interface可以继承，type alias则不行
2. 错误提示方面interface可以直接提示interface名称，更加直观，type alias 并不是总能在错误提示中展示正确的名字。
3. interface只能用来声明对象类型，不能用来为基本类类型\(string,number）重命名，也不能声明基础类型的union类型。

{% hint style="info" %}
Tips: 能使用interface的地方就用interface的方式，否则才使用type alias方式。
{% endhint %}





### **可选类型声明：**

> 可以通过 `x?` 的形式将@变量声明为可选类型，表示该@变量允许为 `null` 或者 `undefined`。

> 该特性一般需要搭配 编译参数设置 `strictNullChecks` 打开编译器的空值检查功能，然后编译器在遇到对可选类型操作时却没做空值判断的情况下会报错。

以下代码展示了如何安全的使用可选类型：

```text
type User = {
  name:string;
}

function printName(user?:User){
  console.log(user.name);    // 报错 Object is possibly 'undefined'

  if(user){
    console.log(user.name);    // OK,通过非空判断编译器可以推导出该逻辑分支下user是非空的。
  }
  
  console.log(user!.name);   // OK ,通过非空断言!告诉编译器user一定非空  
  
  console.log(user?.name);   // OK ,安全调用语法，该语法是JS语法不属于TS范畴，如果user为空，则不会访问name，避免了运行时崩溃。
  
}
```





### **泛型声明：**

> 泛型并非用来声明某个具体类型，而是用来声明不同@变量 之间的类型关联。

> 泛型一般用在 函数，class 或 对象类型声明上。

**泛型函数：**

```text
// 泛型 ，声明了返回值类型与参数数组元素类型一致 的关联关系
function firstElement<Type>(arr: Type[]): Type {
    return arr[0];
}

let first = firstElement([2, 3, 4]);  // first 是 number类型



// 泛型还支持给类型加上约束，以下声明了 Type 类型 是 {length: number} 类型或它的子类型
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}

const longerObj = longest({ length: 1 }, { length: 2 });  //OK,两个对象都有length属性

const longerArray = longest([1, 2], [1, 2, 3]); // OK，数组类型有 length 属性

const longerString = longest("123", "1234");  // OK，string类型也有 length 属性

 
const notOK = longest(10, 100);// 报错，number类型没有 length属性

```

**泛型类：**

```text
// 使用泛的方式定义一个包装类，声明contents属性的类型与构造函数参数类型一致 的关联关系
class Box<Type> {
  contents: Type;
  constructor(value: Type) {
    this.contents = value;
  }
}

const b = new Box("hello!");
let content = b.contents; // content 是string类型
```

**泛型对象类型：**

```text
// 声明一个contents类型是泛型的结构
interface Box<Type> {
  contents: Type;
}

let boxA: Box<string> = { contents: "hello" };

// 泛型方式声明string类型的Array
let strAry:Array<string> = [''];
```

**泛型union类型：**

```text

// 泛型方式声明一个union类型为 空或Type的
type OrNull<Type> = Type | null;


// 泛型方式声明一个union类型为 Type或Type数组
type OneOrMany<Type> = Type | Type[];

let name: OrNull<string> = Math.random() > 0.5 ? "han" : null;// 可为空的string类型

let nameOrNames: OneOrMany<string> = Math.random() > 0.5 ? "han" : ['han'];  // 可以是string数组或string的类型
```



