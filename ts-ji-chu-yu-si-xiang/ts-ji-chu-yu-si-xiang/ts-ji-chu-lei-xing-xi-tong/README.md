---
description: TS中的类型系统
---

# TS基础-类型系统

### 术语表

| 术语 | 描述 |
| :--- | :--- |
| TSC | TS编译器，终端指令名称是tsc |
| 静态类型 | 编译时即可确定类型。 |
| 动态类型 | 运行时才能确定类型。 |



先来看一段JS代码：

```text

function todo (x) {
    x.doSomething();
};

let man = {
    doSomething: function () { }
};

todo(man);

```

分解下上述JS代码：

1. 有一个todo的函数，接收一个参数x，函数实现是调用x的doSomething方法。
2. 创建一个man对象，该对象包含一个doSomething属性，该属性值是一个函数
3. 调用todo方法，参数传入man对象。

目前为止没有什么不妥的地方。

但是这一切运行正常完全是因为调用todo函数的时候我们掌握了todo函数的所有信息，如果说我们调用todo函数时候并不知道该函数需要什么结构的对象，那整个程序就无法正常运行。

比如代码变成以下形式：

```text
let obj = {fn:todo};
obj.fn(/* ？？？ */);
```

我们接收到一个obj对象，该对象有个fn属性。

此时我们应该如何调用fn？

正常情况下我们需要找到obj对象的fn属性被赋值的地方，然后查看被赋值给fn的函数的实现，才能确定我们应该怎么调用fn。

在单人开发的小型项目中，这种查找方式还能接受，但是一旦放到多人协作的项目中，这样将极大降低开发效率，还如容易出错，比如todo函数实现方式某天突然改变了。

以上例子中遇到的问题就是传统JS开发中时刻都会遇到的场景，如果能够在代码运行出错之前就报告问题，那样将会是极好的。

TS静态类型检查功能正是帮忙处理上述这类场景，下面我们把上述代码通过TS改造一下：

步骤如下：

1. 安装TS编译工具TSC ，终端输入`npm install -g typescript`
2. 将前面的js文件后缀名改为.ts
3. 修改js文件中代码如下

```text
function todo (x:{doSomething:Function}) { // TS添加参数类型声明，TS类型语法是后置，跟Java或OC的前置类型语法不一样。
    x.doSomething();
};

let obj = {fn:todo};
obj.fn({}/* ？？？ */);// 
```

1. 运行tsc xxx.ts，然后将看到控制台提示如下：

```text
 Argument of type '{}' is not assignable to parameter of type '{ doSomething: Function; }'.
 Property 'doSomething' is missing in type '{}' but required in type '{ doSomething: Function; }'.
```

TS帮我们发现了这个调用错误，并给出明确的提示：**需要传入一个 { doSomething: Function} 类型的参数**。



到目前为止，可以看到TS可以在不实际运行JS代码的情况下发现这些本应该在运行阶段才会出错的类型问题，但是好像效果也不是很直观，可能有人会想，我直接运行JS代码看报错不也可以发现问题吗？

当然不是！

在简单项目中确实如此，但是实际项目中有大量的场景和逻辑分支，运行JS代码不可能覆盖到所有这些场景和逻辑分支，即使仅大部分覆盖，也将耗费大量的测试精力。而TS直接就可以全部发现这些类型问题。

现在大家对TS的用处应该有了一个比较基础的了解，但是可能还是感觉使用方式太麻烦，需要编译文件，还需要一行行查看控制台输出。

不用担心！

TS在开发工具支持上已经非常完善，目前主流的JS项目开发工具都已经提供了TS的支持，只要在代码编辑工具中加入TS支持，无需自己手动运行tsc命令，便可边写代码边看到编辑工具给出的错误提示，跟平时使用XCode或者AndroidStudio开发时体验非常相似，这在APP开发中看似习以为常的功能，其实在常规JS开发中是缺失的，正是TS补上了这缺失的一块。

**到这，可能你已经迫不及待想跟TS进行更加深入的交流，下面我们就从整体上一起看看TS类型系统的全貌。**

### **TS类型系统：**

> TS类型系统与JS类型系统最大区别是TS是静态类型，JS是动态类型。

> 静态类型：编译时即可确定类型。

> 动态类型：运行时才能确定类型。

先看一段使用TS类型的基本语法：

```text
let myName: string = 'han.yang';  // 声明了一个string类型的变量myName
type User = {                    // 声明一个对象类型并设置别名为User
    name: string,                 // 该对象类型包含一个string类型的name属性 
    readonly gender: 'male' | 'female',   // 该对象类型包含一个'make'|'female' 类型的gender属性,并且是个只读属性

}
// 关于'make'|'female'这种类型是  字面量类型 + union方式的组合类型，下文会讲到


function getMaleUserByName(name: string): User {  // 定义一个函数接收一个string类型的参数，返回一个User类型的对象

    let iUser: User = {      // 声明一个User类型的变量iUser
        name: myName,       // 给user  name属性赋值
        gender: 'male'      // 给user gender属性赋值。
                            // 如果这里给的值不是'male'或'female'，TSC会报错
    }
    // gender是只读属性，一经初始化就不能再被赋值，否则tsc将报错
    // iUser.gender = 'female';

    // 如果赋值给user的对象没有 name 或 gender字段，TSC会报错

    return iUser;
}

// 调用 getMaleUserByName 函数获得一个User对象,赋值给 User类型数组第一个元素。
let user: User[] = [getMaleUserByName(myName)];
```

接下来大家自己跟着下面步骤操作一下：

1. 创建`test.ts`文件，拷贝上述代码，然后使用 `tsc test.ts` 命令编译试试。
2. 你会发现控制台什么都没输出，但是在`test.ts`文件所在目录下多出一个`test.js`文件，该文件即编译后的生成文件,对比下`.ts`和`.js`两个文件的内容区别。
3. 然后再修改`test.ts`文件中的内容，尝试给变量赋值类型不匹配的对象，给函数传类型不匹配的参数或增减参数个数，给只读属性赋值。再运行`tsc test.ts`。
4. 你会发现控制台有报错，但是`test.js`还是正常生成了，赋给变量的值也是修改后的值。这是因为tsc仅做编译检查并报告错误，不会影响最终js文件的生成，TS类型系统经过编译后将完全从js文件中移除，对JS运行时不造成任何影响（即使JS运行后会出错）。
5. 可以通过添加编译参数 `tsc --noEmitOnError test.ts` 告诉`tsc`编译报错的时候不要生成`.js`文件。

{% hint style="info" %}
更多关于tsc的编译参数设置，可以查看[tsc编译选项](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
{% endhint %}



通过上面的例子，大家可以大概了解到TS类型系统在代码开发中起到的作用。

**用一句话概括TS类型系统的作用：**

TS的类型系统对JS开发中关心的的以下几个方面做了约束和检查：

1. 变量的类型
2. 函数返回值的类型
3. 函数的参数类型和个数
4. 对象的成员数量和类型
5. 数组的元素类型
6. 合法的值（上面例子中 gender字段的 合法值是 female或male\)
7. 数据的访问权限 （readonly ，private ，const等修饰符）

{% hint style="info" %}
由于TS的类型检查对 变量，函数参数，对象属性，数组元素，函数返回值都 生效，为了方便表述，后面用 **@变量** 统一指代 变量，函数参数，对象属性，数组元素，函数返回值。用 **@赋值** 统一指代为变量赋值，函数参数传参，对象属性赋值，数组插入元素，函数返回。
{% endhint %}



接下来我们一起来逐项学习TS的类型系统。

