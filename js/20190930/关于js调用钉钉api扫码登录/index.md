js中调用钉钉api，生成二维码并使用钉钉扫码登录分为以下步骤

# 1.   引入钉钉api
```javascript
<script src="https://g.alicdn.com/dingding/dinglogin/0.0.5/ddLogin.js"></script>
```

# 2.   调用初始化函数
```javascript
let url = encodeURIComponent('https://www.baidu.com')
let APPID = '3454kj3n5kj3n53425'
let goto = encodeURIComponent('https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid='+APPID+'&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=' + url)
			
DDLogin({
    id: "login_container",//这里需要你在自己的页面定义一个HTML标签并设置id，例如<div id="login_container"></div>或<span id="login_container"></span>
    goto: goto, 
    style: "border:none;background-color: #fff",
    width: "312",
    height: "290"
});
```
### 其中 **id** 对应的是在页面定义的标签元素 例如
```javascript
<div id="login_container"></div>
```
### **appid**可以在钉钉开发者平台中获取，或者找项目负责人要，没有这个东西是绝对不可能对接成功的

### **goto**  是对应扫码确认之后跳转的页面路径， *一定* 要使用 encodeURIComponent 方法转义

### **width** 和 **height** 是显示二维码区域的宽高， *并非* 二维码实际宽高， 二维码固定宽高为 210px * 210px

### 这个时候，页面上的承载标签内应该就有钉钉的二维码了

### *PS：* 如果是在vue、react等项目里，DDLogin 方法一定要在元素挂在之后再执行，不然会报错导致二维码无法显示：cannot read property 'innerHTML' of undifined,一开始我放在vue 的created钩子函数里面，一直出不来，后看翻看源码发现问题，源码如下
```javascript
//  console.log(DDLogin)
!function (window, document) {
    function d(a) {
        var e, c = document.createElement("iframe"),
            d = "https://login.dingtalk.com/login/qrcode.htm?goto=" + a.goto ;
        d += a.style ? "&style=" + encodeURIComponent(a.style) : "",
            d += a.href ? "&href=" + a.href : "",
            c.src = d,
            c.frameBorder = "0",
            c.allowTransparency = "true",
            c.scrolling = "no",
            c.width =  a.width ? a.width + 'px' : "365px",
            c.height = a.height ? a.height + 'px' : "400px",
            e = document.getElementById(a.id),
            e.innerHTML = "",
            e.appendChild(c)
    }
    window.DDLogin = d
}(window, document);
```

### 可以看出，它通过getElementById 获取了DDLogin中传入的元素，并且将内部元素清空，然后append一个最新生成的用iframe标签包裹的二维码，正是因为如此，在执行DDLogin方法之前，页面就应该存在这个承载元素

# 3.    定义事件处理函数
### 光有个二维码肯定是不够的，接下来就是用户扫描二维码，扫描二维码就会触发事件，作出相应处理
```javascript
let hanndleMessage = (event) => {
    let origin = event.origin;
    if (origin == "https://login.dingtalk.com") { //判断是否来自ddLogin扫码事件。
        let loginTmpCode = event.data; //拿到loginTmpCode后就可以在这里构造跳转链接进行跳转了
        let url2 = "https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid=dingoagmeinihj4esvxigq&response_type=code&scope=snsapi_login&state=STATE&redirect_url=" + url + "&loginTmpCode=" + loginTmpCode
        window.location.href = url2;
        // this.$router.push('/main')   //  vue的路由跳转
    }
};
```
### redirect_url 是用户钉钉扫码并缺点击确定之后跳转的页面，而且必须带上loginTmpCode，不然会被钉钉判定为不合法路径，导致无法跳转

#   4.  将事件挂载在全局
```javascript
if (typeof window.addEventListener != 'undefined') {
    window.addEventListener('message', hanndleMessage, false);
} else if (typeof window.attachEvent != 'undefined') {
    window.attachEvent('onmessage', hanndleMessage);
}
```

### 这样用户在手机扫码，并点击确认之后，浏览器就能接收到消息，触发 onMessage事件，执行下一步操作
#   5.  所有代码
```javascript
let url = encodeURIComponent('https://www.baidu.com')
let APPID = '3454kj3n5kj3n53425'
let goto = encodeURIComponent('https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid='+APPID+'&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=' + url)
			
DDLogin({
    id: "login_container",//这里需要你在自己的页面定义一个HTML标签并设置id，例如<div id="login_container"></div>或<span id="login_container"></span>
    goto: goto, 
    style: "border:none;background-color: #fff",
    width: "312",
    height: "290"
});

let hanndleMessage = (event) => {
    let origin = event.origin;
    if (origin == "https://login.dingtalk.com") { //判断是否来自ddLogin扫码事件。
        let loginTmpCode = event.data; //拿到loginTmpCode后就可以在这里构造跳转链接进行跳转了
        let url2 = "https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid=dingoagmeinihj4esvxigq&response_type=code&scope=snsapi_login&state=STATE&redirect_url=" + url + "&loginTmpCode=" + loginTmpCode
        window.location.href = url2;
        // this.$router.push('/main')   //  vue的路由跳转
    }
};

if (typeof window.addEventListener != 'undefined') {
    window.addEventListener('message', hanndleMessage, false);
} else if (typeof window.attachEvent != 'undefined') {
    window.attachEvent('onmessage', hanndleMessage);
}
```