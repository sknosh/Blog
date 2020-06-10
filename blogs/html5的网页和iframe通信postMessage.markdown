## 实现页面和里面iframe页面之间的通讯
*背景：页面A中通过iframe嵌入B页面,这个两个不同域*

方式一
### A页面要提交消息给B页面
```javascript
iframeEle.contentWindow.postMessage() 
```

### B页面要提交消息给A页面
```javascript
window.parent.postMessage()
```

方式二
*背景：页面A通过调用window.open()打开B页面,可以实现窗口和通过window.open的窗口间的通讯*

### A页面向B页面发送消息
```javascript
const openWindow = window.open() // 新建窗口
// 如果直接openWindow.postMessage()是不行的，要异步才可以的
setTimeout(function(){
  openWindow.postMessage()
})
```

### B页面向A页面发送消息
```javascript
window.opener.postMessage()
```
