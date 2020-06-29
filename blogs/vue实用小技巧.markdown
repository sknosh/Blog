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

