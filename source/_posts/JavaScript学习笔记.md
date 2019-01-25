---
title: JavaScript学习笔记
abbrlink: 20244
date: 2018-11-06 16:28:16
tags: JavaScript
categories: JavaScript
---
## JavaScript高级程序设计

### 5.4：引用类型-函数的内部属性
call: 用来代替另一个对象调用一个方法
apply: 与call一样，区别在于 call 的第二个参数可以是任意类型，而apply的第二个参数必须是数组。
callee: 该属性是一个指针，指向拥有这个 arguments 对象的函数（arguments.callee 指向函数自身）
```javascript
function add(a,b){
    alert(a+b);
}

function sub(a,b){
    alert(a-b);
}

add.call(sub,3,1);
```




> 在函数内部，有两个特殊的对象：`arguments` 和 `this`。其中，它是一个类数组对象，包含着传入函数中的所有参数。虽然 arguments 的主要用途是保存函数参数，但这个对象还有一个名叫 `callee` 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。

```javascript
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * arguments.callee(num-1)
    }
} 
```

caller: 这个属性中保存着调用当前函数的函数的引用，仅在函数在另一个函数内被调用时有效，如果是在全局作用域中调用当前函数，它的值为 null。

```javascript
function a(){ 
    alert(a.caller); 
}

function b(){
    a(); 
}

b();  // a函数弹出function b(){ a(); }
a(); //a函数弹出 null
```


### 6.面向对象的程序设计

> 对象属性前的下划线 _ 是一种常用的记号，用于表示只能通过对象方法访问的属性。 

> 构造函数始终都应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。
