---
description: JS类型系统
---

# JS基础知识-类型系统

### （二）类型系统

> JS类型系统与java或OC有很大区别，JS类型系统仅提供基本类型，而复杂类型都属于JS基本类型中的object类型，因此我们把JS的复杂类型系统称作JS的对象系统，这个后面介绍，这节主要介绍的就是类型系统即基本类型

#### 基本类型

ES标准中定义了8种基础数据类型，JS运行时可以通过`typeof`运算符得到当前数据的类型，比如`console.log(typeof '')` 将得到`'string'`.

| **JS类型** | **typeof运算结果** | **对应包装类\(构造器）** |
| :--- | :--- | :--- |
| `Boolean` | 'boolean' | Boolean |
| `Number` | 'number' | Number |
| `String` | 'string' | String |
| `undefined` | 'undefined' | 无 |
| `null` | 'object' | 无 |
| `BigInt` | 'bigint' | BigInt |
| `Symbol` | 'symbol' | Symbol |
| `Object` | 'object' | Object |

**Typeof :** `typeof`运算符能够在运行时得到数据对应的类型，但function是个例外，function是object类型，但对function类型使用typeof运算结果是function。null也是个例外，null不属于object类型，但是null通过typeof运行得到的是'object'，这也是js开发中经常发生错误的一个地方。**包装类：**JS中类型对应的包装类\(也叫帮助类，或构造器）封装了对该类型数据的操作函数，e.g. String.prototype.toUpperCase\('case'\);



关于类型系统就这么多，下面重点介绍对象系统。

