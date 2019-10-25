#1. 定义一个作为发布订阅载体的对象
```javascript
let event = {
    _events: {}
}
```
###上面代码中，_events 用来存储订阅的事件，为什么是对象形式而不是数组形式呢？
###我们一一道来

#2. 加上订阅的方法
```javascript
let event = {
    _events: {},

    on(key, fn){
        if (!this._events[key]) {
            this._events[key] = []
        }
        this._events[key].push(fn)
    }
}
```
###现在就能解释 _events 为什么是对象了，我们可以订阅多种不同的事件，以对象的形式存储，到时候发布的时候就会方便很多
###如果传进来的 key 原本不存在，我们就应该初始化并添加该类型，然后添加订阅事件
#3. 加上发布的方法
```javascript
let event = {
    _events: {},

    on(key, fn) {
        if (!this._events[key]) {
            this._events[key] = []
        }
        this._events[key].push(fn)
    },

    emit() {
        let key = [].shift.call(arguments)
        let fns = this._event[key]

        if (!fns || fns.length === 0) {
            return false
        }

        fns.forEach(fn => {
            fn.apply(this, arguments)
        })
    }
}
```
###首先我们获取传进来的订阅的类型，如果根本不存在该类型，或者该类型的事件个数为0，就直接return，否则依次执行该类型下订阅的事件
#4. 取消订阅
###就上面的结构已经满足基础的发布订阅了，但是总觉得缺点什么，不如加上取消事件订阅
```javascript
let event = {
    _events: {},

    on(key, fn) {
        if (!this._events[key]) {
            this._events[key] = []
        }
        this._events[key].push(fn)
    },

    emit() {
        let key = [].shift.call(arguments)
        let fns = this._event[key]

        if (!fns || fns.length === 0) {
            return false
        }

        fns.forEach(fn => {
            fn.apply(this, arguments)
        })
    },

    remove(key, fn) {
        let fns = this._event[key]

        if (!fns) {
            return false
        }

        if (!fn) {
            fns && (fns.length = 0)
        } else {
            fns.forEach((cb, index) => {
                if (cb === fn) {
                    fns.splice(index, 1)
                }
            })
        }

    }
}
```
###如果根本没有该类型的订阅，直接return；
###如果没有传入该类型的具体事件，就直接清空该类型的所有事件
###否则，移除该事件