## vue生命周期详解

### beforeCreate：
开始初始化实例，此时那不到data，methods和watch

```javascript
console.log("el:" + this.$el); //undefined
console.log("data:" + this.$data); //undefined 
```
### created：
开始用Object.defineProperty对data里的数据进行监听，可以拿到data，methods和watch，是最早可以拿到实例里的数据和方法的生命周期钩子

```javascript
console.log("el:" + this.$el); //undefined
console.log("data:" + this.$data); //已被初始化 
```

### beforeMount：
挂载开始之前被调用，相关rander函数首次被调用，此时还拿不到真实的dom，但是可以拿到el

```html
<div id="app">
    <ul>
    <li v-for="(item,index) in arr" :key="index">{{item}}</li>
    </ul>
</div>
```
```javascript
console.log("el:" + this.$el); //已被初始化
console.log("data:" + this.$data); //已被初始化 
console.log("data:" + this.$data); //已被初始化 

var app = new Vue({
    data: {
        li: [1,2]
    },
    created() {
    console.log('created',document.querySelectorAll('li').length) //1
    },
    beforeMount() {
    console.log('beforeMount',document.querySelectorAll('li').length) //1
    },
    mounted() {
    console.log('mounted',document.querySelectorAll('li').length) //2
})
```
### mount：
完成挂载，可以拿到真实的dom

### beforeUpdate：
当数据更新后出发的钩子函数，这个钩子函数里拿到的是更改之前的数据，虚拟DOM重新渲染之前被调用，可以在这个钩子中进一步地修改data，这不会触发附加的重渲染过程。

### update：
里拿到更改后的数据，但是并不建议在这里面进行对异步数据得到的dom操作，因为有可能你当前的数据不止更改一次，而update只要相关的数据更改一次就会执行一次，不要在当前钩子里修改当前组件中的data，否则会继续触发beforeUpdate、updated这两个生命周期，进入死循环！

### beforeDestroy： 
实例销毁之前调用，在这里实例仍然可用

### destroyed 
Vue实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。



