# TS访问修饰符

**只读修饰符 readonly 与 const：**

> 用来修饰对象属性或数组元素是只读属性。

readonly 只能修复单个属性：

```text

type User = {
  readonly name: string;
  readonly phone: string;
}
let user: User = { name: 'han', phone: '189' };
user.name = 'yang';  // 报错。Cannot assign to 'name' because it is a read-only property.


```

const可以表示整个对象都是只读（只能用于对象实例）：

```text
type User = {
  name : string;
  phone : string;
};
let user = {name:'han',phone:'189'} as const;
user.name = 'yang';  // 报错。Cannot assign to 'name' because it is a read-only property.
```

只读数组的两种声明方式：

第一种使用readonly 或 const：

```text
let roAry: readonly number[] = [1, 2, 3];
roAry[0] = 101; // 报错! Index signature in type 'readonly number[]' only permits reading.

let constAry = [1, 2, 3] as const;
constAry.push(102); // 报错!Property 'push' does not exist on type 'readonly number[]'
```

第二种使用ReadonlyArray：

```text
let ary: number[] = [1, 2, 3, 4];
let roary: ReadonlyArray<number> = a;

roary[0] = 12; // 报错!Index signature in type 'readonly number[]' only permits reading.

roary.push(5); // 报错!Property 'push' does not exist on type 'readonly number[]'.

ary = roary; // 报错! The type 'readonly number[]' is 'readonly' and cannot be assigned to the mutable type 'number[]'

ary = roary as number[];  // OK. 告诉tsc这个数组是可读写的
ary[1]=0; // OK  ，TS的可读属性仅仅是编译阶段的判断，对运行时并没有影响。
```

{% hint style="warning" %}
试试 `new ReadonlyArray()` 然后看看tsc是否能编译过，看看报什么错，思考下为什么会报错。

答：

不能，报不能创建类型。因为`ReadonlyArray` 是TS的类型，TS类型仅存在于编译过程，编译结束后将会把所有的TS类型从JS代码中擦除，new 针对JS对象的操作符，无法作用在TS类型上。
{% endhint %}



**访问级别修饰符 public，private，protected：**

> 用来修饰class属性的访问级别

> public ：默认级别，公开属性

> private : 私有属性，只有class内部可以访问

> protected ： 受保护的属性，只有class本身和其子类可以访问

```text
class Base {
  public publicX: number = 1;
  protected protectedX: number = 2;
  private privateX: number = 3;
}

class Derived extends Base {
  fn() {
    this.protectedX; // OK
    this.privateX;   // 报错：Property 'priX' is private and only accessible within class 'Base'
  }
}
let obj = new Derived();
obj.publicX;  // OK
obj.protectedX;  // 报错
obj.privateX;  // 报错
```

