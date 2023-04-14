---
title: 关于apply,call,bind理解 
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
tags: [Javascript]
categories: Javascript
date: 2020-02-28 12:15:00
---
## 前言：

首先明确一个基本概念：函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

```javascript
 // 定义一个A对象
let A = {
    color: 'white',
    say(e1,e2) {
        console.log(e1 + ' and ' + e2   + ' is '+ this.color)
    }
}

// 定义一个B对象
let B = {
    color: 'black'
}

// B对象需要用到A对象的方法或属性
A.say.call(B,'hair','eyes')  // =>hair and eyes is black
A.say.apply(B,['hair','eyes'])  // =>hair and eyes is black

```

以上例子说明了：

- call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，即：改变运行时候函数的`this`指向

- 对于 apply、call 二者而言，作用完全一样，只是接受参数的方式不太一样：apply和call接收的第一个参数是都是`this`本身，而第二个参数`apply`接收的是一个数组参数`[1,2,3]`，`call`接收的是列表参数（1,2,3），即：参数个数不确定时使用`apply`，参数个数确定时使用`call`



## 常用用法：

```javascript
// 例子1：数组之间的追加
let A = [1,2,3];
let B = [4,5,6];
Array.prototypr.push.apply(A,B)   // [1,2,3,4,5,6]
Array.prototypr.push.call(A,B)    // [1,2,3,[4,5,6]]

// 例子2：获取数组中的最大值和最小值
let arr = [1,2,3,4,5]
let maxNum = Math.max.apply(Math,arr)
let maxNum = Math.min.call(Math,1,2,3,4,5)

// 例子3：验证是否是数组（前提是toString()方法没有被重写过）
let isArray = (obj) => {
     return Object.prototype.toString.apply(obj) === '[object Array]'
}
 
 // 例子4：类（伪）数组使用数组
let tagName= document.getElementsByTagName("*")
let domNodes = Array.prototype.slice.call(tagName);

```



小结：

- apply、call、bind都是用来改变函数的this对象的指向
- apply、call、bind第一个参数都是this要指向的对象
- apply、call、bind都可以利用后续参数传参
- bind返回的是对应函数；apply、call则是立即调用



补充：

在函数内部，`arguments`是一个`类数组对象`，用于保存函数参数，其内部存在的2个属性：`callee`，`caller`

- `callee:`是一个指针，指向拥有这个`arguments`对象的函数

  ```javascript
  function cool(){
      console.log(arguments.callee);
  }
  
  cool();
  
  //结果打印出来（chrome下）：
  ƒ cool(){
   console.log(arguments.callee);
  }
  ----------------------------------------------------------------------------------------------
  function add(num) {
      if(num <= 1){
          return 1;
      }else{
          return num * arguments.callee(num-1);
      }
  }
  add(10);    //3628800
  ```

- `caller`：保留着调用`当前函数`的`函数`的引用 (全局作用域下调用该函数，caller为null)

  ```javascript
  function add() {
      addChild();
  }
  
  function addChild() {
      console.log(addChild.caller);
  }
  
  add();
  
  //在chrome下打印结果：
  ƒ add() {
    addChild();
  }
  
  ```

  