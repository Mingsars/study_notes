##  设置全局滚动条高度
1.  window.scrollTo()
```javascript
window.scrollTo(0, 0)

window.scrollTo({
  left: 0,
  top: 0
})
```
2.  window.scrollBy()
```javascript
window.scrollBy(0, 0)

window.scrollBy({
  left: 0,
  top: 0
})
```

####    区别：scrollTo是直接移动到对应位置，而scrollBy是移动到相对位置

##  将一个元素设置在视窗最上面
1.  
```javascript
// 获取元素的offsetTop(元素距离文档顶部的距离)
let offsetTop = document.querySelector(".box").offsetTop;

// 设置滚动条的高度
window.scrollTo(0, offsetTop);
```
2.  使用锚点
```javascript
<a href="#box">盒子出现在顶部</a>
<div id="#box"></div>
```
3.  利用scrollIntoView进行展现
```javascript
document.querySelector(".box").scrollIntoView();

//  可传参,start 顶部、center 中央、end 底部 
document.querySelector(".box").scrollIntoView({
  block: 'start' || 'center' || 'end'
});
```

##  设置滚动具有平滑的效果
```javascript
window.scrollTo({
  behavior: "smooth"
});

window.scrollBy({
  behavior: "smooth"
});

document.querySelector(".box").scrollIntoView({
  behavior: "smooth"
});

//  或者使用css
html {
  scroll-behavior: smooth; // 全局滚动具有平滑效果
}

// 或者所有
* {
 scroll-behavior: smooth;
}
```

##  实用的小方法
1.  scrollingElement
```javascript
//  常用的写法
let scrollHeight = document.documentElement.scrollHeight || document.body.scrollHeight;

//  现在可以这么玩
let scrollHeight = document.scrollingElement.scrollHeight;
```

2.  滚动节流
```javascript
window.addEventListener("scroll", throttle(() => console.log("滚动+1")));

function throttle(fn, interval = 500) {
  let run = true;

  return function () {
    if (!run) return;
    run = false;
    setTimeout(() => {
      fn.apply(this, arguments);
      run = true;
    }, interval);
  };
}
```

3.  滚动防抖
```javascript
window.addEventListener("scroll", debounce(() => console.log("滚动结束！")));

function debounce(fn, interval = 500) {
  let timeout = null;

  return function () {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      fn.apply(this, arguments);
    }, interval);
  };
}
```

4.  滚动结束后，强制滚动到指定元素
```css
ul {
  scroll-snap-type: x mandatory;
  
  li {
    scroll-snap-align: start;
  }
}

ul {
  scroll-snap-type: x mandatory;
  
  li {
    scroll-snap-align: center;
  }
}
```



