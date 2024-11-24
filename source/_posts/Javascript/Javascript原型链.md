---
title: Javascript原型链
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
tags: [Javascript]
categories: [Javascript]
date: 2020-02-28 12:15:00
---
> 直接上图！额(⊙o⊙)…，有点乱！不着急，让我们一步步来理解

![img](https://i.loli.net/2019/05/16/5cdd315c45bc895052.png)

首先先来明确这三个属性的定义:

- `prototype`：指向`原型对象`（函数特有属性）
- `__proto__`：指向构造该对象的构造函数的`原型对象`
- `constructor`：指回该原型对象中的构造函数

步骤分析：

1. f1,f2是构造函数Foo()实例化出来的对象，`f1.__proto__`指向其构造函数的原型对象，和其构造函数`Foo.prototype `指向的是同一个原型对象，同时原型对象可以通过`constructor`指回构造函数（其他构造函数同理）
2. Foo函数原型对象使用`Foo.prototype.__proto__ `指向了对象类型的原型对象Object.prototype ：因为Foo原型对象本身也是一个对象，所以使用`__proto__`指向了对象构造函数`Object()`的原型对象`Object.prototype`
3. Object函数原型对象`Object.prototype`是初始化的原型对象了，所以该对象的构造函数的原型对象为空null`Object.prototype.__proto__ == null`，也就是找不到创造它的对象了
4. Object构造函数和其他构造函数的区别就是它的原型对象就是兜底的`Object.prototype`
5. `Foo.__proto__`、`Object.__proto__`、`Function.__proto__`这些构造函数的构造函数都是`Function()`,所以它们的构造函数原型对象都是`Function.prototype`
6. Function构造函数的原型对象`Function.prototype`是通过Object构造函数创建的，所以其原型对象为`Object.prototype`



## 小结

- `__proto__`属性指向构造该对象的构造函数的原型对象：
  - 函数的`__proto__`都指向Object原型对象
  - 对象的`__proto__`都指向其构造函数的原型对象
- `prototype`属性指向`原型对象`：
  - 只有函数才有`prototype`属性
  - `原型对象`存在`constructor`的构造函数，该属性指回其构造函数
- 函数同时也是一个对象：即：`f.prototype.__proto__  == f.__proto__.__proto__`也就是图中的1+2 == 5+ 6，最终都是指向Object原型对象
- `prototype`、`__proto__`两个最终都是指向同一个原型对象
- Object原型对象的`__proto__`的属性值为null
- 任何函数最终的构造函数都是指向`函数构造函数Function`，任何对象最终的构造对象原型都是`对象原型对象Object.protutype`

> 嘿嘿~当时，只有我和上帝两个人知道写的是什么，而现在只有上帝一个人知道了！



参考链接：http://www.cnblogs.com/smoothLily/p/4745856.html