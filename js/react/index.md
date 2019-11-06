#   基于脚手架搭建react项目
## 1. 首先看看本地有没有脚手架工具，打开命令行
```javascript
//  命令行直接输出，按回车
create-react-app
```
####    会显示如下
```javascript
Please specify the project directory:
  create-react-app <project-directory>

For example:
  create-react-app my-react-app

Run create-react-app --help to see all options.
```
####    这样就代表已经安装了脚手架工具
####    如果没有安装的话先下载
```javascript
//  npm
npm install create-react-app -g

//  cnpm
cnpm install create-react-app -g

//  yarn
yarn add create-react-app -g

//  有时会提示权限相关问题，windows用管理员权限打开命令行，mac加上sudo
sudo cnpm install create-react-app
```
## 2.  创建新项目
```javascript
create-react-app my-react-app
```
#### create-react-app 是指令，my-react-app 是项目名称
#### 如果想加上typescript
```javascript
create-react-app my-react-app --typescript
```

## 3.  运行项目
```javascript
cd my-react-app
yarn start
```

## 4.  加上路由
```javascript
yarn add react-router-dom -D

//  如果是基于typescript，加上@type/，不然会报错
yarn add @type/react-router-dom -D
```

### 4.1 路由表
#### 从router4 开始，react将 <b>不再</b> 推荐集中式路由管理
```javascript
//  基于typescript
//  一级路由
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'
import React from 'react';
import ReactDOM from 'react-dom';

interface IProps{}
interface IState{}

export default class App extends React.Component<IProps, IState> {
    constructor(props: any) {
        super(props)
        this.state = {}
    }

    render() {
        return (
            <Router>
                <Route path="/main" component={Main} />
                <Route path="/user" component={User} />
                <Route path="/shop" component={Shop} />
            </Router>
        )
    }
}

ReactDOM.render(<App />, document.getElementById('root'))
```
####    如果要启用二级路由，就去各个一级路由组件里加上二级路由
```javascript
import { Link, Route, Switch } from 'react-router-dom'

interface IProps{
    match: any
}
interface IState{}

export default class Main extends React.Component<IProps, IState> {
    constructor(props: any) {
        super(props)
        this.state = {}
    }

    render() {
        return (
            <Link to="staff">staff</Link>
            <Link to="functionList">functionList</Link>
            <Router>
                <Route path={this.props.match.url + '/staff'} component={Staff} />
                <Route path={this.props.match.url + '/functionList'} component={FcuntionList} />
            </Router>
        )
    }
}
```
####    如果要启动三级路由，同样是在二级路由里面配置，但是有一点不同
```javascript
import { Link, Route, Switch } from 'react-router-dom'

interface IProps{
    match: any
}
interface IState{}

export default class Staff extends React.Component<IProps, IState> {
    constructor(props: any) {
        super(props)
        this.state = {}
    }

    render() {
        return (
            <Link to={{pathname: "/main/staff/staffList"}}>staffList</Link>
            <Link to={{pathname: "/main/staff/operation"}}>operation</Link>
            <Router>
                <Route path={this.props.match.url + '/staffList'} component={StaffList} />
                <Route path={this.props.match.url + '/operation'} component={Operation} />
            </Router>
        )
    }
}
```

####    上面的代码中，Link 标签中的 to 传入的值将不再是一个简单的下一级路由，而是改成对象格式，pathname写入完整的路由