# TS模块规范



> TS支持多种JS的模块规范，这里我们只讨论ES6的模块规范，即`import` 与 `export`。

> TS中的类型可以与js的值一样使用`import` 导入其他模块的类型，或者`export` 导出自己模块的类型。

> 不过为了让Babel等转译器更好的识别并移除掉TS代码，TS还扩展了`import type`关键字来指明导入的是TS类型。

> 示例代码如下：

**User.ts文件**

```text
export interface User{
  name:string;
};
```

**test.ts文件**

```text
// import {User} from './hello';

// 上面方式导入也OK，但是使用下面这个方式更规范。
import type {User} from './hello';
let user:User = {name:'han'};
```

 至此，关于TS类型系统的关键内容都已经介绍完毕，下面我们来总结一下这套系统的设计思想。

