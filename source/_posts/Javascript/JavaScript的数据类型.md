---
title: JavaScript数据类型
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [学习笔记]
categories: Javascript
date: 2020-02-28 12:15:00
---

## JavaScript的数据类型

> 按照类型来分有**基本数据类型**和**引用数据类型**

**基本数据类型：**`String`、`Number`、`Boolean`、`Null`、`Undefined`、`Symbol`

**引用数据类型：**`Object`【Object是个大类，表示一种无序键值对的集合：function函数、array数组、date日期...等都归属于Object】

<img src="C:\Users\90856\Documents\思维导图\JavaScript的数据类型.png" alt="JavaScript的数据类型"  />





## JavaScript的内置对象_RegExp(正则表达式)

> 正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript 中，正则表达式也是对象。这些模式被用于 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp) 的 [`exec`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 和 [`test`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) 方法，以及 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String) 的 [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)、[`matchAll`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)、[`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)、[`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) 和 [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) 方法。下面介绍 JavaScript 正则表达式。

RegExp中的方法：

- test():  

  检索正则表达式与指定的字符串是否匹配。返回 `true` 或 `false`。

- exec(): 

  在一个指定字符串中执行一个搜索匹配。返回一个结果`数组`或 `null`

- toString():  

  返回一个表示该正则表达式的字符串

String中用到正则的方法有：

- match():  

  检索返回一个字符串匹配正则表达式的结果

- replace():  

  返回一个由替换值（`replacement`）替换部分或所有的模式（`pattern`）匹配项后的新字符串。                   原字符串不会改变

- search():  

- split():  



match: 提取匹配项

注意：`.match` 语法是目前为止一直使用的 `.test` 方法中的“反向”，即：[`String.prototype.match()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)

```js
'string'.match(/regex/);
/regex/.test('string');
```

多种模式匹配:  `/a|b|c/`

精准匹配：`/abc/`

范围匹配：`/[0-9a-z]/`

字符集：`/q[abc]n/`

字符组：`/P(engu|umpk)in/`

否定字符集：[ ^0-9abc ] 

[g]()：全局匹配

[i]() ：忽略大小写 `/Aa/i`



[+]()：匹配一次或多次的字符

[*]()：匹配出现零次或多次的字符

[?]()：匹配零个或一个的字符



[?]()：懒惰匹配 `/a+?/`

[i]()：贪婪匹配`/a+i/`（默认）

> tip: 用于设置数量符号`*`、`+`的匹配模式



[$](/story$/): 搜寻字符串末尾的匹配模式：`/Cal$/`

[^](/story$/): 

1. 搜寻字符串开始的匹配模式：`/^Cal/`
2. 创建否定字符集 `[^0-9aeiou]`

### 元字符

`\w`：匹配大写字母和小写字母以及数字，等同于`[A-Za-z0-9_]`

`\W`：匹配模式是非字母数字字符，等同于`[^A-Za-z0-9_]`

`\d`：匹配数字字符，等同于元字符 `[0-9]`

`\D`：匹配非数字字符，等同于元字符 `[^0-9]`

`\s`：匹配空格、回车符、制表符、换页符和换行符，等同于元字符 `[ \r\t\f\n\v]`

`\S`：匹配非空白字符，等同于元字符 ` [^ \r\t\f\n\v]`

` . ` ： 匹配任何一个字符

[{}]()：数量说明符 

1. 指定匹配模式的上下限`/a{3,5}/`
2. 只指定匹配的下限`/a{3,}/`
3. 指定匹配的确切数量`/a{3}/`



**正向先行断言**：正向先行断言会查看并确保搜索匹配模式中的元素存在，但实际上并不匹配。 正向先行断言的用法是 `(?=...)`，其中 `...` 就是需要存在但不会被匹配的部分

**负向先行断言**：负向先行断言会查看并确保搜索匹配模式中的元素不存在。 负向先行断言的用法是 `(?!...)`，其中 `...` 是希望不存在的匹配模式。 如果负向先行断言部分不存在，将返回匹配模式的其余部分

```js
// 在正则表达式 pwRegex 中使用先行断言以匹配大于 5 个字符且有两个连续数字的密码
let sampleWord = "astronaut";
let pwRegex = /(?=\w{6})(?=\w*\d{2})/; 
pwRegex.test(sampleWord); // => true
```



**捕获组重用模式**: 捕获组是通过把要捕获的正则表达式放在括号中来构建的。 

在下面这个例子里， 目标是捕获一个包含数字字符的词，所以捕获组是将 `\d+` 放在括号中：`/(\d+)/`

```js
//在reRegex中使用捕获组来匹配一个只由相同的数字重复三次组成的由空格分隔字符串。
let repeatNum = "42 42 42";
let reRegex = /^(\d+)\s\1\s\1$/;
repeatNum.match(reRegex) // => [ '42 42 42','42']
```



**使用捕获组搜索和替换**：

```js
//使用三个捕获组编写一个正则表达式 `fixRegex`，这三个捕获组将搜索字符串 `one two three` 中的每个单词。 然后更新 `replaceText` 变量，以字符串 `three two one` 替换 `one two three`，并将结果分配给 `result` 变量。 确保使用美元符号（`$`）语法在替换字符串中使用捕获组。
let str = "one two three";
let fixRegex = /([a-z]{3,})\s([a-z]{3,})\s([a-z]{3,})/; // 用于匹配的字符串或正则表达式
let replaceText = '$3 $2 $1'; // 用于替换匹配的字符串
str.replace(fixRegex, replaceText); // => three two one
```





## JavaScript的对象

![JS原型链详细图解](C:\Users\90856\Documents\Typora笔记\images\JS原型链详细图解.png)



**`constructor`:** 

> 该属性是对创建这个实例(f)的`构造函数`(F)的一个引用



**`__proto__`:** 

> 该属性是对实例对象(f)所在原型链上的`原型对象`(F.prototype)的引用,即：f._ _proto_ _ 与 F.prototype 指向同一个原型对象
>
> **tip**: 
>
> - 实例对象与其原型对象处于同一原型链，所以对象可以向上查找使用该原型对象的属性和方法，即：可以沿着`__proto__`逐层查找
>
> - 自定义构造函数(F)在JS中也是一种特殊的对象,所以F的`__proto__`指向构造函数(Function)的原型对象，即`Foo.__proto__ === Function.__proto__ === Function.prototype`



**`instanceof`：**

> 用于检测构造函数(F)的 `prototype` 属性是否出现在某个实例对象(f)的原型链上。 

```js
let f = new F()
f instanceof F //=> true
```







## 编程方式

### 命令式编程

### 函数式编程

### 面向对象编程





