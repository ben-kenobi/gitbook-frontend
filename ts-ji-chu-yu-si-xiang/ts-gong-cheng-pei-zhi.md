---
description: 以TSRN工程为例
---

# TS工程配置



## RN项目接入TS

RN项目中使用TS步骤主要分为以下三个：

1. 安装TS相关依赖库
2. 配置tsconfig.json文件
3. 安装TS相关的开发工具
4. 开始使用



### 项目配置

* 项目根目录下使用yarn增加以下依赖：

```
yarn add --dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer
```

上面命令是往项目中添加了 `typescript` 库（该库主要功能是tsc编译工具，如果全局安装过TS库，项目中可以不用安装该库，不过为了开发人员间统一，一般项目中也安装该库），还有`jest , react , react-native, react-test-renderer` 这4个库对应的TS类型声明库。



* 创建一个`jest.config.js`文件来配置 Jest 以使用 TypeScript:

```
module.exports = {
  preset: 'react-native',
  moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
};
```



### tsconfig.json

> `tsconfig.json`文件是TS项目的配置文件，主要配置两块内容：**需要TS编译的文件** 和 **TS编译选项**。

> 一个包含`tsconfig.json`文件的目录就表示这个目录是一个TS项目的根目录。

> 使用 `tsc --init` 在当前目录下创建`tsconfig.json`文件。

> TS社区还为不同类型的项目编写了基础的TS配置，项目开发人员可以继承这些基础配置，来简化项目配置。

来看一下RN项目使用RN的基础TS配置步骤：

1. RN项目中导入TS基础配置库：`yarn add --dev @tsconfig/react-native`
2. 在`tsconfig.json`中继承该基础配置，`tsconfig.json`文件将长下面这样：

```
{
  "extends": "@tsconfig/react-native/tsconfig.json",
  "compilerOptions": {
    //...
  },
  "include": ["src/**/*"]
}
```

一起来看看`@tsconfig/react-native/tsconfig.json` 的内容

```
{
  "$schema": "https://json.schemastore.org/tsconfig",
  "display": "React Native",
  "compilerOptions": {
    "target": "esnext",
    "module": "commonjs",
    "lib": [
      "es2017"
    ],
    "allowJs": true,
    "jsx": "react-native",
    "noEmit": true,
    "isolatedModules": true,
    "strict": true,
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "skipLibCheck": false
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

可以看到RN项目的基础配置设置了`noEmit:true`,这表示`tsc`后不输出编译后的目标JS文件。

这是因为RN项目开发过程只使用了TS的类型检查功能，并不需要每次都编译都输出JS文件来验证代码的运行状态。

<mark style="color:red;">那RN项目中的TS代码是在开发到运行过程中的哪个阶段被转换成JS代码的呢？大家思考一下。</mark>

{% hint style="info" %}
tsconfig.json支持上百种配置，有需要的时候可以查阅[tsconfig配置项](https://www.typescriptlang.org/tsconfig#)
{% endhint %}





### TS工具介绍

> TS在开发工具上的支持非常全面，大部分JS开发工具都支持TS特性。

> 我们这里只针对介绍下vsCode上的TS工具。

vsCode是微软开源的一款代码编辑器，可以通过安装插件成为功能强大的IDE工具。

不过vsCode内置了支持TS的工具，无需额外安装插件。

vsCode可以自动识别`.ts`和`.tsx`文件，然后在编辑器上提供基于TS的类型检查，自动补全等功能。

vsCode还会自动递归查找当前文件目录或其父目录下的`tsconfig.json`文件，根据该配置中的编译配置来对代码进行检查。

### 开始使用

使用TS的项目，所有`.js`文件后缀需要改成`.ts`,`.jsx`需要改成`.tsx`。

运行 `yarn tsc` 来检查TS文件中的类型错误，当然这种方式需要在控制台操作，很低效，开发中一般安装编辑器TS插件，达到在在代码编辑器中实时查看类型错误。

##
