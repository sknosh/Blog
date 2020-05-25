## 水平居中

### 行内块水平居中
```css
.container {
    text-align: center;
}
.item {
    display: inline-block;
}
```
### 表格table水平居中
```css
.container {
    
}
.item {
    display: table;
    margin: 0 auto;
}
```
### margin: 0 auto水平居中
```css
.container {
    
}
.item {
    width: 100px;
    margin: 0 auto;
}
```
### flex水平居中
```css
.container {
    display: flex;
    justify-content: center;
}
.item {

}
```

### 绝对定位居中
```css
.container {
    width: 100%;
    position: relative;
}
.item{
    position: absolute;
    left: 50%;
    transition: translateX(-50%);
    /* margin-left: -50px; */
    width: 100px;
    height: 100px;
}
```

## 垂直居中

### flex垂直居中
```css
.container {
    display: flex;
    align-items: center;
}
.item {

}
```
### 绝对定位垂直居中
```css
.container {
    position: relative;
}
.item {
    position: absolute;
    top: 50%;
    transition: translateY(-50%);
    /* margin-top: -50px; */
    width: 100px;
    height: 100px;
}
```
### flex垂直居中
```css
.container {
    display: flex;
    align-items: center;
}
.item {

}
```

### vertical-align垂直居中
```css
.container {
    height: 100px; //要设置高，或者行高才会生效
    display: table-cell;
    /* display:inline-block; */
    vertical-align: middle;
}
.item {

}
```

## 水平垂直居中
```css
.container{
    display:table-cell;
    vertical-align: middle;
    text-align: center;
}
.item{
    display: inline-block;
}
```