# TS中的基础类型



### **与JS基础类型对应的TS基础类型：**

ES标准中定义了8种基础数据类型，下表列出了这些数据类型在TS中的对应类型

| **JS类型** | **typeof运算结果** | **对应包装类型** | **TS对应类型** |
| :--- | :--- | :--- | :--- |
| `Boolean` | 'boolean' | Boolean | boolean |
| `Number` | 'number' | Number | number |
| `String` | 'string' | String | string |
| `undefined` | 'undefined' | 无 | undefined |
| `null` | 'object' | 无 | null |
| `Object` | 'object' | Object | object |
| `BigInt` | 'bigint' | BigInt | bigint |
| `Symbol` | 'symbol' | Symbol | symbol |

> **Typeof :** `typeof`运算符只能对以上几种基本类型数据进行区分，其他复杂类型数据最终运算结果都是`'object'`。js中所有可以使用new关键字的类型在运行时都是Object类型，比如Array，Map，Date都是Object类型，对这些类型变量使用`typeof`运算结果都是`'object'`，function也是object类型，但是function是个例外，对function类型使用typeof运算结果是function。null也是个例外，null不属于object类型，但是null通过typeof运行得到的是'object'，这也是js开发中经常发生错误的一个地方。

> **包装类型：**JS中类型对应的包装类型封装了对该类型数据的操作函数，e.g. String.prototype.toUpperCase\('case'\);

> **TS对应类型：**JS是动态类型，声明变量时候并不能给变量指定类型，TS为JS扩展了静态类型，因此TS针对JS这些基础类型都定义了相应的静态类型





### **TS基础扩展类型：**

> 上层类型：可以理解成父类型

> 下层类型：可以理解成子类型

| **TS基础扩展类型** | **说明** |
| :--- | :--- |
| `unknown` | 最上层的类型，表示是某个类型，但是还不知道具体类型，任何类型是unkown类型，但是unkown类型需要断言才能转换成其他类型。 |
| `never` | 最下层的类型，表示运行时永远都不可能出现的类型。never类型的值可以赋值给任何类型，但是任何类型的值都无法赋值给never类型。 |
| `void` | 一般用作返回值表示没有返回值。 |
| `any` | 指任意类型，被指定为any类型的变量可以赋值给任意类型，any类型变量也可以接受任意类型数据。等于说TS编译器不对any类型操作做任何检查 |
| `Function` | 表示最上层的函数类型，所有函数类型都是Function类型。 |
| literal types | 字面量值也可以作为一种类型，这种类型一般作为枚举使用 ，比如 字面量 'male' 用在类型上就是指'male'类型，只接受值为'male' 的赋值。 |



