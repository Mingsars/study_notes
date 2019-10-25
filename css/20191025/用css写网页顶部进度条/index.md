#1. 网页顶部的进度条？
## 第一眼看到这个需求，第一反应是用js监听浏览器滚动高度，配合css实现，那么如果不能使用js呢？真让人头大。
#2. 纯css实现
##1. 将设网页包裹在body里面，可以滚动的是整个body，我们如下设置样式
```css
body {
    background-image: linear-gradient(to right top, #ffcc00 50%, #eee 50%);
    background-repeat: no-repeat;
}
```
### 这个时候，我们能看到body的背景色，已经弄模拟出进度条的韵味了
##2. 运用伪元素，把多出来的部分遮住
```css
body::after {
    content: "";
    position: fixed;
    top: 5px;
    left: 0;
    bottom: 0;
    right: 0;
    background: #fff;
    z-index: -1;
}
```
###这里的 z-index:-1 很关键，不然会挡住内容
##3. 结束了？
###上面的属性设置完，我们就能看到几个基本的进度条样式了，可是随着往下滚动，就发现不对劲，进度条到一半就卡住了？
###是的，卡住了，因为背景色一直延伸到body的最下面，而进度条滚动的高度是达不到body自身的高度的，所以我们要限制
###body背景图的高度
##4. 限制背景图高度
```css
body {
    background-image: linear-gradient(to right top, #ffcc00 50%, #eee 50%);
    background-size: 100% calc(100% - 100vh + 5px);
    background-repeat: no-repeat;
}
```