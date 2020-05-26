## Vue的事件机制原理

### vue中提供了四个事件API，分别是$on，$once，$off，$emit

思路：在vue上创建一个公共的事件数组event存放事件，通过增删数组订阅事件，遍历数组对象触发事件

[on](#on)

[emit](#emit)

[off](#off)


---
<span id="on"></span>
### on

```javascript
vue.prototype.$on(event, fn) {
    const vm = this
    // 如果是数组的时候，则递归$on
    if(Array.isArray(event)) {
        for(let i = 0; i < event.length; i++) {
            vm.$on(event[i], fn)
        }
    } else {
        (vm._events[event] || (vm._events[event] = [])).push(fn)
    }
}
```

<span id="emit"></span>
### emit

```javascript
vue.prototype.$emit(event, fn) {
    // 取第一个参数
    const event = [].shift.call(arguments)
    const vm = this
    let cbs = vm._events[event]
    if (!cbs || cbs.length === 0) {
        return
    } else {
        for(let i = 0; i < cbs.length; i++) {
            cbs[i].apply(vm, arguments)
        }
    }
}

```

<span id="off"></span>
### off

```javascript
vue.prototype.$off(event, fn) {
    const vm = this
    // 取第一个参数
    const event = [].shift.call(arguments)
    if (Array.isArray(event)) {
        for (let i = 0; i < event.length; i++) {
            vm.$off(event[i], fn)
        }
    }
    if (event.length === 1) {
        vm._events[event] = {}
    } else {
        for (let i = 0; i < event.length; i++) {
            vm._events.splice(i, 1)
        }
    }
    
}
```
