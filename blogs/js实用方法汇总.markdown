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

*引用类型转换为字符串会优先自动调用toString如果没有调用就valueOf，引用类型转换为数字会优先自动调用valueOf如果没有就调用toString*

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
console.log(add(1)(2)(3)(4)(6));   //10
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
let union = new Set([...arr1].filter((item) => {arr2.！has(item)})); // {1}
```

### rem原理

### tcp协议

### 1px问题

