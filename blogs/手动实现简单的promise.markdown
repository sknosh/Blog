```javascript
function promise(fuc) {
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
        fuc(resolve)
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