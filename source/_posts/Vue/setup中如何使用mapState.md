---
title: setup中如何使用mapState
excerpt: false
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
tags: [vue3]
categories: Vue
date: 2020-02-28 12:15:00
---



> vuex提供了mapState、mapMutations…辅助函数，让我们能够方便的映射vuex中的state、getters、mutations以及actions

```js
computed: {
	...mapState(['name', 'age']),
	// otherComputed...
}
```

> mapState函数实际生成的是一个如下的对象，我们将其展开到组件的配置中

```js
computed: {
	name: function(){
		return this.$store.state.name
	},
	age: function(){
		return this.$store.state.age
	}
    // ...otherComputed
}
```


但是setup中没有computed选项，的计算属性只能使用computed组合式api进行创建，如下

```js
import { useStore } from 'vuex'
// store等同于this.$store，setup中没有this，只能这个书写
const store = useStore()
const name = computed(() => store.state.name)
const age = computed(() => store.state.age)
```

因此我们只需要将mapState返回的对象改造为setup的组合式API写法即可解决这个问题

```js
// 此处省略依赖导入...
const stateObj = mapState(['name', 'age'])
/*
// stateObj结果
{
    name: function(){
        return this.$store.state.name
    },
    age: function(){
        return this.$store.state.age
    }
}
*/
const myState = {}
for (const key in stateObj) {
    if (Object.hasOwnProperty.call(stateObj, key)) {
        // fn函数中需要使用到this.$store，因为setup中没有this，因此需要手动修改该函数的this指向
        const fn = stateObj[key].bind({ $store: useStore() })
        myState[key] = computed(fn)
    }
}
// 然后就可以正常使用我们仓库的数据了
const { name, age } = myState
console.log(name.value, age.value);
```

## 优化

>  每次这么写会很麻烦，不仅会使键盘的寿命缩减，还容易把手上磨出茧子，因此我们可以对这些东西进行封装

```JS
// 省略导入语法
function setupMapState (keys) {
    // 使用仓库对象
    const $store = useStore()
    // 根据参数进行映射
    const stateFn = mapState(keys)
    const res = {}
    for (const key in stateFn) {
        if (Object.hasOwnProperty.call(stateFn, key)) {
            // 修改计算函数内部this指向
            const fn = stateFn[key].bind({ $store })
            // 存储
            res[key] = computed(fn)
        }
    }
    // 将映射结果返回
    return res
}
```


大功告成，接下来就可以正常使用了

```js
const { name, age } = setupMapState(['name', 'age'])
console.log(name.value, age.value);
```



## vuex的模块化

> 如果vuex进行了模块化拆分，我们在进行映射时需要传递对应的模块名，显然现在的一个参数难以满足我们的需求，展开运算符可以帮上大忙

```JS
// 省略导入语法
function setupMapState (...arg) {
    // 使用仓库对象
    const $store = useStore()
    // 根据参数进行映射
    const stateFn = mapState(...arg)
    const res = {}
    for (const key in stateFn) {
        if (Object.hasOwnProperty.call(stateFn, key)) {
            // 修改计算函数内部this指向
            const fn = stateFn[key].bind({ $store })
            // 存储
            res[key] = computed(fn)
        }
    }
    // 将映射结果返回
    return res
}
```


这样就能支持其他模块的导入了

```js
const { name, age } = setupMapState(['name', 'age'])
console.log(name.value, age.value);
const { name: otherModuleName } = setupMapState('otherModule' ,['name']) 
console.log(otherModuleName.value);
```

## 最终版本

>  其他的映射函数也可以进行处理，这样就更加方便了

```JS
import { computed } from 'vue'
// 相关辅助函数导入
import { mapActions, mapGetters, mapMutations, mapState, useStore } from 'vuex'
function mapAll (keys, mapFn) {
    // 导入仓库对象
    const $store = useStore()
    // 根据传入的辅助函数和其他参数进行映射
    const stateFn = mapFn(...keys)
    // 记录映射结果
    const res = {}
    // 如果映射的是state或getters需要使用computed组合式API包裹，因此做了这样一个判断
    const isMapData = [mapState, mapGetters].includes(mapFn)
    for (const key in stateFn) {
        if (Object.hasOwnProperty.call(stateFn, key)) {
            // 修改映射函数内部this指向
            const fn = stateFn[key].bind({ $store })
            // 记录：state或getters使用computed进行包裹，其他的直接记录
            res[key] = isMapData ? computed(fn) : fn
        }
    }
    // 返回结果
    return res
}
// 导出对应的setup映射函数
export function setupMapState (...keys) {
    return mapAll(keys, mapState)
}
export function setupMapMutations (...keys) {
    return mapAll(keys, mapMutations)
}
export function setupMapGetters (...keys) {
    return mapAll(keys, mapGetters)
}
export function setupMapActions (...keys) {
    return mapAll(keys, mapActions)
}
```


使用说明: 封装完成之后只需要进行导入，然后和使用正常的辅助函数用法相同

```js
// 映射state
const { name } = setupMapState(['name', 'age'])
// 其他模块的state
const { name: homeName } = setupMapState('home', ['name'])
// 映射mutations
const { setName } = setupMapMutations(['setName'])
// 映射getters
const { names } = setupMapGetters({
  names: 'getName'
})
// 映射actions
const { postName } = setupMapActions(['postName'])
// 传参方式和vuex辅助函数相同
```





