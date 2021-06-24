---
description: JS基础语法
---

# JS基础知识-基础语法

### （一）基本语法

> JS语法上与java类似，下面简单看一下JS的语法，对JS大致语法有个了解即可

#### 语句

**变量声明与赋值语句：**

> JS有三种变量声明方式，let ，var ，const。var是旧版本的JS使用，现在基本不用，这里不做介绍，有兴趣可以自己查询var与let的区别。

```text
let str = 'hello world';  // 赋值语句
str = 'hi world';     // 允许重新赋值
const conStr =  'unchangeable hello world';  // 常量声明
conStr = 'hi world';  // 报错
```

**语句块:**

```text
{
    let str = 'hello world';
}
console.log(str);   // 打印str，为undefine，因为str作用域仅限于语句块内
```

### 

**流程控制语句：**

> JS支持if else条件判断语句，

> swtich 语句，

> while， do while，for 循环语句，

> 以上语句语法和行为方式都与java一致。

> JS特有的 for of 与 for in 循环，我们一起来看一下。

```text
let ary = ['a','b','c'];

for (let value of ary) {
    console.log(value);   //  a  , b ,c 
}

for (let idx in ary) {
   console.log(idx);   //  0 , 1, 2
}
```

#### 运算符与表达式

**条件运算符与条件表达式**

> 与java一样，有&gt; , &lt; ,&gt;= ,&lt;=, == ,!= , && ,\|\| 等, JS 特有的条件运算符有 === ，instanceof 等。

```text
let str1 = 'hello';
let str2 = new String('hello');

console.log(str1 == str2);   // true

console.log(str1 === str2);   // false


console.log(str1 != str2);   // false

console.log(str1 !== str2);   // true

console.log(str2 instanceof String); // true
```

**数学运算符与数学表达式**

> 与java基本类似，支持 +-\*/和位运算等,我们主要看一下字符串的拼接方式

```text
let str1 = 'hello';
let str2 = 'world';
console.log('say : '+str1 + ' ' + str2);   // 通过 + 号拼接
console.log(`say : ${str1} ${str2}`);      // 通过 ``  嵌套  ${} 方式拼接
```

#### 

大概了解基本语法后，我们看下JS的类型系统。

