#   1.  什么是 **Web Component**
####    组件是当下前端的流行，也是未来很长一段时间的趋势，Web Component API 是谷歌公司在chrome浏览器推动的原生组件，
####    相比第三方框架，原生组件更简单、直接，不用加载其他模块。

#   2.  使用方法
##  a.  自定义元素需要使用javascript定义一个类
```javascript
class UserCard extends HTMLElement {
    constructor() {
        super()
    }
}
```
####    上面代码中，UserCard 就是自定义元素的类，由于父类是HTMLElement，因此继承了HMTL元素的特性。

##   b. 使用浏览器原生的customElements.define()方法告诉浏览器我们定义的<user-card>元素与这个类关联
```javascript
window.customElements.define('user-card', UserCard)
```

##  c.  自定义元素的内容
####    自定义的元素 <user-card> 目前还是空的，下面在里面给这个元素加入内容
```javascript
class UserCard extends HTMLElement {
    constructor() {
        super()

        //  记住一定写在构造函数 constructor里面
        var image = document.createElement('img');
        image.src = 'https://semantic-ui.com/images/avatar2/large/kristy.png';
        image.classList.add('image');

        var container = document.createElement('div');
        container.classList.add('container');

        var name = document.createElement('p');
        name.classList.add('name');
        name.innerText = 'User Name';

        var email = document.createElement('p');
        email.classList.add('email');
        email.innerText = 'yourmail@some-email.com';

        var button = document.createElement('button');
        button.classList.add('button');
        button.innerText = 'Follow';

        container.append(name, email, button);
        this.append(image, container);
    }
}
```
####    上面代码中，this.append()，this指向的是自定义元素实例
####    完成这一步之后，自定义元素的内部DOM结构就生成了

##   c.  优化写法
####    不难发现，使用javascript写上一节的结构很麻烦，好在 Web Component API 提供了 template 标签
```javascript
//  写在body标签里即可
<template id="userCardTemplate">
  <img src="https://semantic-ui.com/images/avatar2/large/kristy.png" class="image">
  <div class="container">
    <p class="name">User Name</p>
    <p class="email">yourmail@some-email.com</p>
    <button class="button">Follow</button>
  </div>
</template>
```
####    这样定义之后，我们就可以很方便的改写上面我们定义的类
```javasscript
class UserCard extends HTMLElement {
  constructor() {
    super();

    let templateElem = document.getElementById('userCardTemplate');
    let content = templateElem.content.cloneNode(true);
    this.appendChild(content);
  }
}  
```
####    上面代码中，我们使用了 templateElem.content.cloneNode(true) 方法，克隆了它所有的子元素，使用该方法的原因，可以同步联想到对象的深拷贝，可能存在多个实例，我们不能改变它本身，导致其他的实例也被改改变

##  d.  目前为止所有的代码
```javascript
<body>
  <user-card></user-card>
  <template>...</template>

  <script>
    class UserCard extends HTMLElement {
      constructor() {
        super();

        var templateElem = document.getElementById('userCardTemplate');
        var content = templateElem.content.cloneNode(true);
        this.appendChild(content);
      }
    }
    window.customElements.define('user-card', UserCard);    
  </script>
</body>
```

##  e.  添加样式
####    很显然，这样定义一个元素，并且使用，是没有任何样式的，我们需要为它添加样式
```javascript
<template id="userCardTemplate">
    <style>
        :host {
            display: flex;
            width: 300px;
            height: 140px;
        }
        .image {
            width: 100%
        }
        ...
    </style>
    <img src="https://semantic-ui.com/images/avatar2/large/kristy.png" class="image">
    <div class="container">
        <p class="name">User Name</p>
        <p class="email">yourmail@some-email.com</p>
        <button class="button">Follow</button>
    </div>
</template>
```
####    上面的:host伪类，指代的是自定义元素本身
####    像这样写在temlate里面，只会对该自定义元素起作用，可以参考vue-cli 中样式的scoped理解

##  f.  没卵用？
####    就目前看来，好像作用并不大，内容全部是写死的，还不如直接去掉定义直接：
```javascript
<img src="https://semantic-ui.com/images/avatar2/large/kristy.png" class="image">
<div class="container">
    <p class="name">User Name</p>
    <p class="email">yourmail@some-email.com</p>
    <button class="button">Follow</button>
</div>
```
####    接下来我们遮掩改，让他开始更像一个组件，加上了props,并去掉template里面需要动态取值的内容
```javascript
<template id="userCardTemplate">
  <style>...</style>

  <img class="image">
  <div class="container">
    <p class="name"></p>
    <p class="email"></p>
    <button class="button">Follow John</button>
  </div>
</template>

<user-card
  image="https://semantic-ui.com/images/avatar2/large/kristy.png"
  name="User Name"
  email="yourmail@some-email.com"
></user-card>
```
####    这里我们传入了三个变量：image、name、email，然后要在内部接收这几个变量
```javascript
class UserCard extends HTMLElement {
  constructor() {
    super();

    var templateElem = document.getElementById('userCardTemplate');
    var content = templateElem.content.cloneNode(true);
    content.querySelector('img').setAttribute('src', this.getAttribute('image'));
    content.querySelector('.container>.name').innerText = this.getAttribute('name');
    content.querySelector('.container>.email').innerText = this.getAttribute('email');
    this.appendChild(content);
  }
}
window.customElements.define('user-card', UserCard);    
```
#   3.  Shadow DOM
####    有时候我们不希望用户能看到 <user-card> 的内部代码，Web Component API 允许内部代码隐藏起来，这叫做Shadow DOM
```javascript
class UserCard extends HTMLElement {
  constructor() {
    super();
    var shadow = this.attachShadow( { mode: 'closed' } );

    var templateElem = document.getElementById('userCardTemplate');
    var content = templateElem.content.cloneNode(true);
    content.querySelector('img').setAttribute('src', this.getAttribute('image'));
    content.querySelector('.container>.name').innerText = this.getAttribute('name');
    content.querySelector('.container>.email').innerText = this.getAttribute('email');

    shadow.appendChild(content);
  }
}
window.customElements.define('user-card', UserCard);
```
####    上面代码中，this.attachShadow() 方法的参数 {mode: 'closed'}，就表示shadow dom是封闭的，不允许外部访问

#   4.  添加交互（如点击）
```javascript
this.$button = shadow.querySelector('button');
this.$button.addEventListener('click', () => {
  // do something
});
```
####    局部代码
```javascript
class UserCard extends HTMLElement {
  constructor() {
    super();
    var shadow = this.attachShadow( { mode: 'closed' } );

    var templateElem = document.getElementById('userCardTemplate');
    var content = templateElem.content.cloneNode(true);
    content.querySelector('img').setAttribute('src', this.getAttribute('image'));
    content.querySelector('.container>.name').innerText = this.getAttribute('name');
    content.querySelector('.container>.email').innerText = this.getAttribute('email');

    shadow.appendChild(content);

    this.$button = shadow.querySelector('button');
    this.$button.addEventListener('click', () => {
    // do something
    });
  }
}
window.customElements.define('user-card', UserCard);
```