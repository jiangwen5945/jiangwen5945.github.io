---
title: 实现一个简单的vue
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
tags: [Vue]
categories: [Vue]
date: 2022-10-28 12:15:00
---

首先我们创建一个`index.html`文件，引入自定义的`my-vue.js`，然后实例化对象，并传人对象参数

```html

<div id="app">
  <h1>{{ str }}</h1>
  <h2>{{ name }}</h2>
  <input type="text" v-model="name">
  <button @click="handleClick">按钮</button>
</div>


<script src="./my-vue.js"></script>
<script>
  new Vue({
    el: '#app',
    data: {
      str: '你好🎉',
      name: 'jiangwen'
    },
    methods:{
      handleClick(e){
        this.str = 'hello👋'
        console.log(this.str, this.$data.str);
      }
    }
  })
</script>
```

 创建一个Vue类

```js
class Vue {
    constructor(options) {
      this.$data = options.data
      this.$el = document.querySelector(options.el)
    }
}
```

## 模版解析

```js
class Vue {
    constructor(options) {
      this.$data = options.data
      this.$el = document.querySelector(options.el)
+     this.compile(this.$el)
    }
+  	compile(node) {
+      node.childNodes.forEach(e => {
+            // 节点类型
+            if (e.nodeType === 1) {
+                if (e.childNodes.length > 0) {
+                    this.compile(e) //递归调用
+                }
+            }
+            // 文本类型
+            if (e.nodeType === 3) {
+                const reg = /\{\{(.*?)\}\}/g
+                const text = e.textContent
+                e.textContent = text.replace(reg, (match, vmKey) => {
+                    vmKey = vmKey.trim()
+                    return this.$data[vmKey]
+                })
+            }
+        })
+    }
}
```

## 生命周期

```js
class Vue {
    constructor(options) {
        // 1. 实例创建前钩子🪝
        if (typeof options.beforeCreate === 'function') {
            options.beforeCreate.bind(this)()
        }
        // 创建数据 
        this.$data = options.data
        this.proxyData()
        this.observe()
        this.$watchEvent = {}
        this.$methods = options.methods
        // 2. 实例创建完成钩子🪝
        if (typeof options.created === 'function') {
            options.created.bind(this)()
        }
        // 3. dom挂载前钩子🪝
        if (typeof options.beforeMount === 'function') {
            options.beforeMount.bind(this)()
        }
        // 挂载dom
        this.$el = document.querySelector(options.el)
        // 模版解析
        this.compile(this.$el)
        // 4.dom挂载完成钩子🪝
        if (typeof options.mounted === 'function') {
            options.mounted.bind(this)()
        }
    }
}
```

## 添加事件

```js
class Vue {
    constructor(options) {
        ...
    }
    compile(node) {
        node.childNodes.forEach(e => {
            // 节点类型
            if (e.nodeType === 1) {
                if (e.childNodes.length > 0) {
                    this.compile(e)
                }
+                // 判断节点元素是否有@click
+                if (e.hasAttribute('@click')) {
+                    console.log(e.getAttribute('@click'));
+                    // 获取事件名称
+                    const vmKey = e.getAttribute('@click').trim()
+                    e.addEventListener('click', event => {
+                        const fn = this.$methods[vmKey].bind(this)
+                        fn(event)
+                    })
+                }
            }
            // 文本类型
            if (e.nodeType === 3) {
               ...
            }
        })
    }
}
```

## 数据驱动视图更新

```js
class Vue {
    constructor(options) {
        // 1. 实例创建前钩子🪝
        if (typeof options.beforeCreate === 'function') {
            options.beforeCreate.bind(this)()
        }
        // 创建数据 
        this.$data = options.data
+       this.proxyData()
+       this.observe()
+       this.$watchEvent = {}
        this.$methods = options.methods
        // 2. 实例创建完成钩子🪝
        if (typeof options.created === 'function') {
            options.created.bind(this)()
        }
        // 3. dom挂载前钩子🪝
        if (typeof options.beforeMount === 'function') {
            options.beforeMount.bind(this)()
        }
        // 挂载dom
        this.$el = document.querySelector(options.el)
        // 模版解析
        this.compile(this.$el)
        // 4.dom挂载完成钩子🪝
        if (typeof options.mounted === 'function') {
            options.mounted.bind(this)()
        }
    }
    compile(node) {
        node.childNodes.forEach(e => {
            // 节点类型
            if (e.nodeType === 1) {
               ...
            }
            // 文本类型
            if (e.nodeType === 3) {
                const reg = /\{\{(.*?)\}\}/g
                const text = e.textContent
                e.textContent = text.replace(reg, (match, vmKey) => {
                    vmKey = vmKey.trim()
+                   if(this.hasOwnProperty(vmKey)) {
+                       let watcher = new Watcher(this, vmKey, e, 'textContent')
+                        if(this.$watchEvent[vmKey]){
+                          this.$watchEvent[vmKey].push(watcher)
+                       }else{
+                           this.$watchEvent[vmKey] = []
+                           this.$watchEvent[vmKey].push(watcher)
+                       }
+                   }
                    return this.$data[vmKey]
                })
            }
        });
    }
    // 同步数据（保持vue实例对象属性和data中的属性同步，可以直接使用this.xxx获取数据）
+    proxyData() {
+        for (const key in this.$data) {
+            Object.defineProperty(this, key, {
+                get() {
+                    return this.$data[key]
+                },
+                set(val){
+                    this.$data[key] = val
+                }
+            })
+        }
+    }
    // 观察者(监控this.$data的所有值)
+    observe(){
+        for (const key in this.$data) {
+           let value = this.$data[key]
+           const _this = this
+          Object.defineProperty(this.$data, key, {
+                get(){
+                    return value
+                },
+                set(val){
+                    value = val
+                    if(_this.$watchEvent[key]){
+                        // 所有使用该属性值的元素都执行节点更新
+                        _this.$watchEvent[key].forEach((item, index) => {
+                            item.update()
+                        })
+                    }
+               }
+           })
+        }
+    }
+}

+ class Watcher {
+    constructor(vm, key, node, attr){
+        this.vm = vm
+        this.key = key
+        this.node = node
+        this.attr = attr
+    }
+    // 更新节点的内容
+    update(){
+        this.node[this.attr] = this.vm[this.key]
+   }
+ }
```

## 双向绑定

```js
class Vue {
    constructor(options) {
        ...
    }
    compile(node) {
        node.childNodes.forEach(e => {
            // 节点类型
            if (e.nodeType === 1) {
 								...
+               // 判断节点元素是否有v-model
+               if(e.hasAttribute('v-model')){
+                   const vmkey = e.getAttribute('v-model').trim()
+                   if(this.hasOwnProperty(vmkey)){
+                       e.value = this[vmkey] // 将数据赋值给有v-model的节点
+                   }
+                   // 监听元素input事件，赋值
+                   e.addEventListener('input', (event)=>{
+                       this[vmkey] = e.value
+                   })
+               }
            }
            // 文本类型
            if (e.nodeType === 3) {
               ...
            }
        })
    }
}
```



