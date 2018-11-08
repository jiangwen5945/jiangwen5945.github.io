---
title: JavaScript学习笔记
abbrlink: 20244
date: 2018-11-06 16:28:16
tags:
---
## JavaScript高级程序设计

### 5.4：引用类型-函数的内部属性 P_114

callee: 该属性是一个指针，指向拥有这个 arguments 对象的函数
> 在函数内部，有两个特殊的对象：`arguments` 和 `this`。其中，它是一个类数组对象，包含着传入函数中的所有参数。虽然 arguments 的主要用途是保存函数参数，但这个对象还有一个名叫 `callee` 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。

```
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * arguments.callee(num-1)
    }
} 
```

caller: 这个属性中保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为 null。
