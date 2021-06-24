# TS中类型的确定方式



看完TS类型系统的类型和声明类型的方式后，接下来看看TS类型系统是通过哪些方式在代码中确定 @变量 的类型来判断类型是否匹配的。

### **手动指定：**

> TS代码上指定明确的类型

```text
let a:string = '';   // 明确指定 a 是 string类型
```

### **自动推导：**

> tsc还能通过代码上下文自动推导出 @变量 的类型

> 代码上没有明确指定类型且缺少可供推导的上下文信息的时候，TS会默认将变量类型推导为`any`类型，即不对其做任何类型检查。（可以通过编译参数`noImplicitAny`设置，在隐式推导为any的时候报错）。

```text
// 自动推导场景1
let a = '';  
a = 2; // 报错，编译器通过上下文推导出 a 是string类型，不能接受number类型数据


// 自动推导场景2
type strOrNum = string | number;

function print(id: strOrNum) {
  let str = id.toUpperCase(); // 报错，Property 'toUpperCase' does not exist on type 'number'
  if (typeof id === "string") {
    console.log(id.toUpperCase());  // OK，编译器从上下文(typeof id === "string")推导出 当前逻辑分支中 id 是string类型
  } else {
    console.log(id.toUpperCase());// 报错，编译器从上下文推导出 当前逻辑分支中 id 是number类型,number没有toUpperCase方法
  }
}
```

> TS 自动推导类型功能还支持从更加复杂的上下文中推导出数据类型，这里不一一列举。

### **断言指定：**

> 断言属于手动声明方式的一种特殊情况，通过断言可以明确告诉TS编译器某数据符合某类型。

> 断言有类型转换断言与非空断言。

> **类型转换断言：**语法是 `x as Type` 或 `<Type>x`。

> 类型转换断言只能将当前类型转换成更加具体或更加抽象的类型，完全不相关的两个类型无法互相转换。

> **非空断言：**语法是 x!

> 非空断言一般在与[可选类型](https://anker-in.feishu.cn/docs/doccn18rCmzhv6q6LrpBb6X7XUf#mn37YX)搭配使用

类型转换断言：

```text
type User = {
  name: string;
}

let userWithPhone = {
  name: 'han',
  phoneNum: '186'
}

let a: User = userWithPhone;
let phoneNum = a.phoneNum; // 报错，User没有phoneNum属性


phoneNum = (a as { name: string, phoneNum: string }).phoneNum; // OK,通过断言告诉编译器 a 类型是 {name:string,phoneNum:string}



phoneNum = (a as { phoneNum: string }).phoneNum; // 报错,两个完全不同的类型(属性没有重叠) 无法通过断言转换。




let num = 1;
let str: string = num as string;  // 报错，string 与 num 是完全不同的两个类型，无法通过断言转换
str = (num as unknown) as string; // OK。 上文提到unkonwn是所有类型最上层的类，
// 因为不相关的两种类型无法互相转换，所以可以通过将不相关的两个类型先转换成unkonwn再转成指定类型，来跳过这层检查

str = <string><unknown>num    // 类型转换断言的另一种写法
```

非空断言：

```text
type User = {
  name:string;
}

function printName(user?:User){
  console.log(user.name);   // 报错 Object is possibly 'undefined'
  console.log(user!.name);   // OK ,通过!告诉编译器user一定非空
}
```

