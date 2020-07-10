## js实用方法汇总
---

### Array.from
*Array.from是把类数组对象，转换成真正的数组，比如argument*

q创建固定长度的多维数组

```javascript
    // const arr [10][2]
    const arr = Array.from(new Array(10), () => new Array(2))
```
q创建一个包含从0到99的连续整数的数组

```javascript
    // 后面的那个回调函数跟map类似
    const arr = Array.from(new Array(99), (item, index) => index)
```

qString转Array
    
```javascript
    Array.from('string'); 
    // [ "s", "t", "r", "i", "n", "g" ]
```



### fill固定值填充数组

```javascript
    var fruits = ["h", "a", "p", "p", "y"]
    fruits.fill("o"); // ["o", "o", "o", "o", "o"]
```



### 对象转数组的方法

call方法

```javascript
[].slice.call(arguments); //让对象有数组的方法
```

三点运算符号

```javascript
[...arguments]
```


### 实现add((1)(2)(3)) = 1+2+3

思路： 通过闭包保存上一次的计算结果

- 引用类型转换为字符串会优先自动调用toString如果没有调用就valueOf
- 引用类型转换为数字会优先自动调用valueOf如果没有就调用toString
- 引用类型转换为方法，下面函数仅仅是调用了result而不是result()，打印出来是下面结果，其实这里调用valueOf,如果返回不是原始类型，将继续调用toString
```javascript
function add(a) {
    function result(b) {
        a += b
        return result
    }
  
    console.log(result)
}

// 调用add
add
// 打印结果：
ƒ add(a) {
    function result(b) {
        a += b
        return result
    }
  
    console.log(result)
}
```
解题：
```javascript
function add(a) {
    function result(b) {
        a += b
        return result
    }
    // 如果不加toString返回的是函数
    result.toString = function () {
        return a;
    }
    return result
}
add(1)(2)(3)
function add(a) {
    function fun(b) {
    a += b;
    return fun;
    }
    fun.toString = function () {
    return a;
    }
    return fun;
}
console.log(add(1)(2)(3)(4)(6));   //16
```

### Object.create(proto, [propertiesObject])
*返回一个新的对象，它的原型上会有proto属性，proto可以是对象，null，对象的原型*

```javascript
const people = { name: '小明' }
const newPeople = Object.create(people)
console.log(newPeople)  // {}
console.log(newPeople.__proto__) // { name: '小明' }
```

### Object.create(null)跟{}的区别
Object.create(null)没有继承任何原型方法，而{}的继承了object的方法

### new Set()方法
*有add，clear（删除全部键值对，没返回），delete，has，forEach返回类数组对象，可用于去重，并集，交集等*

去重

```javascript
let arr = [1,2,3,3,1,4];
Array.from(new Set(arr)); // [1, 2, 3, 4] 数组去重
[...new Set('ababc')].join('')  //abc 字符串去重
```

并集

```javascript
let arr1 = new Set([1, 2, 3]);
let arr2 = new Set([4, 3, 2]);
let union = new Set([...arr1, ...arr2]); // {1, 2, 3, 4}
```

交集

```javascript
let arr1 = new Set([1, 2, 3]);
let arr2 = new Set([4, 3, 2]);
let union = new Set([...arr1].filter((item) => {arr2.has(item)})); // {2, 3}
```

差集

```javascript
let arr1 = new Set([1, 2, 3]);
let arr2 = new Set([4, 3, 2]);
let union = new Set([...arr1].filter((item) => {arr2.!has(item)})); // {1}
```

### ~~
~~它代表双非按位取反运算符，比Math.floor()更快的方法

```javascript
~~null = 0
~~undefined = 0
~~-1.2 = -1
~~ 1.2 = 1
~~false = 0
~~true = 1
```

### tcp协议

*参考阮一峰老师*

以太网协议、ip协议、tcp协议、http协议

- 以太网协议：解决局域网内的点对点通信
- ip协议： 解决多个局域网直接的通信
- tcp协议： 保证数据通信的完整性和可靠性，防止丢包
- http协议： 应用层协议


TCP的seq：

一个包的大小为1400个字节，一次性发送大量数据，就必须分成多个包，发送的时候，TCP 协议为每个包编号（SEQ），以便接收的一方按照顺序还原,万一发生丢包，也可以知道丢失的是哪一个包。

第一个包的编号是一个随机数，假设为1号包，假定这个包的负载长度是100字节，那么可以推算出下一个包的编号应该是101。这就是说，每个数据包都可以得到两个编号：自身的编号，以及下一个包的编号。接收方由此知道，应该按照什么顺序将它们还原成原始文件。

TCP的ack：

携带信息:
- 期待要收到下一个数据包的编号
- 接收方的接收窗口的剩余容量

默认情况下，接收方每收到两个 TCP 数据包，就要发送一个确认消息。"确认"的英语是 acknowledgement，所以这个确认消息就简称 ACK。

tcp传输图：

![](./image/tcp/tcp.png)


tcp数据包遗失处理：

每一个数据包都带有下一个数据包的编号。如果下一个数据包没有收到，那么 ACK 的编号就不会发生变化，这会导致大量重复内容的 ACK

如果发送方发现收到三个连续的重复 ACK，或者超时了还没有收到任何 ACK，就会确认丢包，从而再次发送这个包。

通过这种机制，TCP 保证了不会有数据包丢失。


### Promise.all 其中某个请求出现异常
Promise.all 其中某个请求出现异常，在then中就会直接返回reject的值

```javascript
function a() {
    return Promise.all([
        Promise.reject(1),
        Promise.resolve(2),
        Promise.resolve(3)
    ])
}
a.then(res => {
    console.log(res)
}, err => {
    console.log(err) // 1
})

// 用catch方法改变状态
function a() {
    return Promise.all([
        Promise.reject(1),
        Promise.resolve(2),
        Promise.resolve(3)
    ].map(p => p.catch(e => e)))
    // map的每一项都是promise, catch方法返回值会被promise.resolve()包裹，这样promise.all的数据都是resolve状态的
    .then(res => {
        console.log(res) // [1,2,3]
    }, err => {
        console.log(err) 
    })
}
a()

// 还可以这样去捕获：
function a() {
    return Promise.all([
        // .then返回的也是promise对象，返回promise对象后就变成resolve状态了
        Promise.reject(1).then(res => {console.log(res)}, err => {console.log(err)}),
        Promise.resolve(2).then(res => {console.log(res)}, err => {console.log(res)}),
        Promise.resolve(3).then(res => {console.log(res)}, err => {console.log(res)})
    ])
    // map的每一项都是promise, catch方法返回值会被promise.resolve()包裹，这样promise.all的数据都是resolve状态的
    .then(res => {
        console.log(res) // [1,2,3]
    }, err => {
        console.log(err) 
    })
}
a()
```