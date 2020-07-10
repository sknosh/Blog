### hook
hook就是在某个时机执行相应代码，在vue里面的生命周期也是hook，在Vue组件中监听生命周期的方法有三种：

1.在组件中加生命周期
``` javascript
this.vm = {
    mounted() {
    }
}
```

2.用过$on,$once去监听所有的生命周期钩子函数

``` javascript
this.$on('hook:updated', () => {})
```

3.在模板中通过@hooks:created这种形式

```javascript
    <Table @hook:updated="handleTableUpdated"></Table>
```
### mixin

mixin可用于局部组件和全局组件中，使用方式是新建一个mixin.js文件，里面写new Vue里面的部分（created，data，methds等），然后再需要用到的页面引入这个文件(例如import {mixin} from "./mixin/mixin")，然后mixin: [minxin]这样使用就可以了

注意点：
- mixin可以定义公共的变量或者方法，但是mixin中的数据是不共享的，也就是每个组件中的mixin实例都是不一样的，都是单独存在的个体，不存在相互影响的
- mixin混入对象值为函数的同名函数选项将会进行递归合并为数组，两个函数都会执行，只不过先执行mixin中的同名函数
- mixin混入对象值为对象的同名对象将会进行替换，都优先执行组件内的同名对象，也就是组件内的同名对象将mixin混入对象的同名对象进行覆盖

### Vue observable
解决多组件状态共享问题，之前Vuex文档介绍，能不用Vuex尽量不用，不然代码看起来繁琐冗余，Vue2.6新增Vue observable，可以作为最小化的跨组件状态存储器，用于简单的场景

创建一个 store.js，包含一个store和一个 mutations，分别用来指向数据和处理方法

```javascript
// store.js
import Vue from 'vue';

export let store = Vue.observable({count: 0, name: '小米'})
export let mutations = {
    setCount (count) {
        store.count = count
    },
    changeName (name) {
        store.name = name
    }
}

```

```html
// index.vue
<template>
    <div>{{count}}</div>
    <button @click="setCount(count+1)">+1</button>
</template>
<script>
    import {store, mutations} from '@/store'
    export default {
        computed: {
            count() {
                return store.count
            }
        },
        methods: {
            setCount:mutations.setCount   
        }
    }
</script>
```

### Vue.extend是一个全局api，一般用于开发公共组件


### require.context实现前端工程自动化
require.context函数可以获取一个特定的上下文，主要是用于实现自动化导入模块

>场景：当一个js里面需要手动引入过多的其他文件夹里面的文件时


```javascript
// directory：要扫描的目录
// useSubdirectories： 是否扫描所以子级文件
// regExp：要扫描的文件，用正则匹配
const context = require.context(directory, useSubdirectories, regExp)
context.keys().forEach(key => {
    // context(key)获取相应的单个文件，default表示export.default导出的内容
    component = context(key).default
    // 安装vue组件
    Vue.component(component.name, component)
})

```
