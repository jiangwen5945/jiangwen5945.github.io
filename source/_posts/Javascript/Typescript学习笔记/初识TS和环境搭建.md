---
title: Typescript学习笔记
tags: [Typescript]
categories: Typescript
thumbnail: https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/11.webp
date: 2020-01-01 18:00:00
---

> TypeScript是一种由微软开发的开源编程语言,它是JavaScript的一个超集,具有以下特性：

##### TypeScript 是静态类型

> 类型系统按照「类型检查的时机」来分类，可以分为`动态类型和静态类型`。

`动态类型`是指在运行时才会进行类型检查，这种语言的类型错误往往会导致运行时错误。JavaScript 是一门解释型语言[[4\]](http://ts.xcatliu.com/introduction/what-is-typescript.html#link-4)，没有编译阶段，所以它是`动态类型`，以下这段代码在运行时才会报错：

```js
let foo = 1;
foo.split(' ');
// Uncaught TypeError: foo.split is not a function
// 运行时会报错（foo.split 不是一个函数），造成线上 bug
```

`静态类型`是指编译阶段就能确定每个变量的类型，这种语言的类型错误往往会导致语法错误。TypeScript 在运行前需要先编译为 JavaScript，而在编译阶段就会进行类型检查，所以 **TypeScript 是静态类型**，这段 TypeScript 代码在编译阶段就会报错了：

```ts
let foo = 1;
foo.split(' ');
// Property 'split' does not exist on type 'number'.
// 编译时会报错（数字没有 split 方法），无法通过编译
```

你可能会奇怪，这段 TypeScript 代码看上去和 JavaScript 没有什么区别呀。

没错！大部分 JavaScript 代码都只需要经过少量的修改（或者完全不用修改）就变成 TypeScript 代码，这得益于 TypeScript 强大的[类型推论][]，即使不去手动声明变量 `foo` 的类型，也能在变量初始化时自动推论出它是一个 `number` 类型。

完整的 TypeScript 代码是这样的：

```ts
let foo: number = 1;
foo.split(' ');
// Property 'split' does not exist on type 'number'.
// 编译时会报错（数字没有 split 方法），无法通过编译
```

##### TypeScript 是弱类型

> 类型系统按照「是否允许隐式类型转换」来分类，可以分为强类型和弱类型。

以下这段代码不管是在 JavaScript 中还是在 TypeScript 中都是可以正常运行的，运行时数字 `1` 会被隐式类型转换为字符串 `'1'`，加号 `+` 被识别为字符串拼接，所以打印出结果是字符串 `'11'`。

```js
console.log(1 + '1');
// 打印出字符串 '11'
```

TypeScript 是完全兼容 JavaScript 的，它不会修改 JavaScript 运行时的特性，所以**它们都是弱类型**。

作为对比，Python 是强类型，以下代码会在运行时报错：

```py
print(1 + '1')
# TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

若要修复该错误，需要进行强制类型转换：

```py
print(str(1) + '1')
# 打印出字符串 '11'
```

> 强/弱是相对的，Python 在处理整型和浮点型相加时，会将整型隐式转换为浮点型，但是这并不影响 Python 是强类型的结论，因为大部分情况下 Python 并不会进行隐式类型转换。相比而言，JavaScript 和 TypeScript 中不管加号两侧是什么类型，都可以通过隐式类型转换计算出一个结果——而不是报错——所以 JavaScript 和 TypeScript 都是弱类型。

> 虽然 TypeScript 不限制加号两侧的类型，但是我们可以借助 TypeScript 提供的类型系统，以及 ESLint 提供的代码检查功能，来限制加号两侧必须同为数字或同为字符串[[5\]](http://ts.xcatliu.com/introduction/what-is-typescript.html#link-5)。这在一定程度上使得 TypeScript 向「强类型」更近一步了——当然，这种限制是可选的。

这样的类型系统体现了 TypeScript 的核心设计理念[[6\]](http://ts.xcatliu.com/introduction/what-is-typescript.html#link-6)：在完整保留 JavaScript 运行时行为的基础上，通过引入静态类型系统来提高代码的可维护性，减少可能出现的 bug。


## 安装/使用Typescript

安装
```bash
npm install -g typescript
```
编写一个`hello.ts`文件

```tsx
function sayHello(person: string) {
    return 'Hello, ' + person;
}

let user = 'Tom';
console.log(sayHello(user));
```

执行手动编译

```bash
tsc hello.ts
```

这时候会生成一个编译好的文件 `hello.js`：

```js
function sayHello(person) {
    return 'Hello, ' + person;
}
var user = 'Tom';
console.log(sayHello(user));
```

在 TypeScript 中，我们使用 `:` 指定变量的类型，`:` 的前后有没有空格都可以。

上述例子中，我们用 `:` 指定 `person` 参数类型为 `string`。但是编译为 js 之后，并没有什么检查的代码被插入进来。

这是因为 **TypeScript 只会在编译时对类型进行静态检查，如果发现有错误，编译的时候就会报错**。而在运行时，与普通的 JavaScript 文件一样，不会对类型进行检查。

#### VSCode自动编译

1. 在项目中执行`tsc --init`生成配置文件
2. 修改配置文件`tsconfig.json`中的一些配置


```json
{
  "compilerOptions": {
	...
    "target": "es2016",                        /* 指定编译的JS版本 */
    "outDir": "./js",                          /* 编译后的js输出目录 */
    "strict": false,                           /* 是否启用严格模式 */
  }
}

```

3. 从 vscode 中的 `终端 > 运行任务 ` 选择 ` tsc:监视 -tsconfig.json`执行自动编译

![image-20230320123710535](https://jiangwen-markdown-img.oss-cn-fuzhou.aliyuncs.com/image-20230320123710535.png) 

#### 运行编译结果

vscode中安装插件`Code Runner`，在编译后的js文件中右键选择 Run Code ,可以在控制台查看运行结果；如果想直接在ts文件中运行，查看结果的话，可以需要通过安装`npm install -g ts-node`，否则结果输出会乱码

