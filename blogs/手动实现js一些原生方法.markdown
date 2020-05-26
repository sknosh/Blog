## 实现一些原生的js方法

[promise方法](#promise)

[call方法](#call)

[new方法](#new)

---

<span id="promise"></span>
### promise方法

```javascript
function promise(fn) {
    this.status = 'padding'
    this.data
    this.reason
    this.callback = []
    function resolve(value) {
        this.status = 'resolve'
        this,callback.push(value)
        this.data = value
    }
    function reject(reason) {
        this.status = 'reject'
        this.reason = reason
    }
    try{
        fn(resolve)
    } catch(e) {
        reject(e)
    }
}
promise.prototype.then((successFn, failure) => {
    if (this.status === 'resolve') {
        successFn(this.value)
    } else if (this.status === 'reject') {
        failure(this.reason)
    }
})
let a = new promise((resolve, reject) => {
  resolve(10)
})
a.then(resolve => {
  console.log(resolve)
})

```

<span id="call"></span>
### call方法

```javascript
function people() {
    this.name = '小明'
    thia.age = '20'
}
function run() {
    console.log(`${this.name}在奔跑`) 
}
```
>把上面的run方法打印出"小明在奔跑"

思路： 把run方法的this指向people就可以了，变成下面这个样子就可以了

```javascript
    function people(age) {
        this.name = '小明'
        thia.age = age
        function run() {
            console.log(`${this.age}岁的${this.name}在奔跑`)
        }
    }
```

调用的时候是通过run.call(people),所以创建一个call方法

```javascript
Function.prototype.myCall(target) {
    let args = []
    target.fn = this
    // 传参数
    for (let i = 1; i < arguments.length; i++) {
        args.push(arguments[i])
    }
    const result = target.fn(...args)
    delete target.fn(...args)
    return result
}
people.myCall(run, '20')
```
<span id="new"></span>
### new方法

思路：创建一个新对象，把实例对象上的属性和方法在这个新对象的环境走一遍，返回新的属性

```javascript

function Dog(){
  this.name = '小明'
}
Dog.prototype.sayName = function(){
  console.log(this.name)
}
Dog.prototype.hhh = function(){
  console.log(this.name)
}
// 上面是本身Dog
function _new(fn,...args){   // ...args为ES6展开符,也可以使用arguments
  //先用Object创建一个空的对象,
  const obj = Object.create(fn.prototype)  //fn.prototype代表 用当前对象的原型去创建
  console.log(obj.hhh)
  //现在obj就代表Dog了,但是参数和this指向没有修改
  const rel = fn.apply(obj,args)
  //正常规定,如何fn返回的是null或undefined(也就是不返回内容),我们返回的是obj,否则返回rel
  return rel instanceof Object ? rel : obj
}
var _newDog = _new(Dog,'这是用_new出来的小狗')
console.log({_newDog})

```