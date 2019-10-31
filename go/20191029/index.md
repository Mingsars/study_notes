
#   Go语言基础结构
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world")
}
```
### 包声明、引入包、函数、变量、语句 & 表达式、注释

## 1. package main
#### &emsp;&emsp;第一行代码 package main 定义了包名。 *必须* 在源文件中非注释的第一行代码中指明该文件属于哪个包。
#### &emsp;&emsp;package main 表示一个可独立执行的程序，每个Go程序都必须包含一个名为 main 的包

## 2. import "fmt"
#### &emsp;&emsp;这一行代码表示告诉Go编译器这个程序需要 fmt 包，类似 es6（js） 中的模块导入 import

## 3. func main
#### &emsp;&emsp;这一行是定义一个函数，类似js中的 function main，不同的是，{ 必须跟在括号后面，保证同一行
#### &emsp;&emsp;main函数是每一个可执行的程序必须包含的，一般来说都是程序启动后第一个执行的函数，如果有init函数则会先执行该函数

## 4. fmt.Println("hello, world")
#### &emsp;&emsp; fmt.Println() 可以讲字符串输出到控制台，类似 console.log() ，不同的是前者会在最后自动加上换行符\n

#   Go语言数据类型
##  1. 布尔
##  2. 数字
##  3. 字符串
##  4. 派生类型
-   指针（Pointer）
-   数组
-   结构化
-   Channel
-   函数
-   切片
-   接口
-   map

#   Go语言变量
```go
package main

import "fmt"

func main() {
    var a string = "hello"
    fmt.Println(a)

    var b, c int = 1, 2
    fmt.Println(b, c)
}
```
##  1. 变量声明
### &emsp; 1. 指定变量类型，如果没有初始化，则变量默认为零值
```go
package main
import "fmt"
func main() {

    // 声明一个变量并初始化
    var a = "RUNOOB"
    fmt.Println(a)

    // 没有初始化就为零值
    var b int
    fmt.Println(b)

    // bool 零值为 false
    var c bool
    fmt.Println(c)
}
```
### &emsp; 2. 根据值自行判定数据类型
```go
package main
import "fmt"
func main() {
    var d = true
    fmt.Println(d)  //  true
}
```

### &emsp; 3. 省略var，但 := 左侧必须有新的变量生命，否则编译错误
```go
package main 

import "fmt"

func main () {
    num := "newnew"
}
```

## 2. 多变量声明
```go
package main

var x, y int
var (  // 这种因式分解关键字的写法一般用于声明全局变量
    a int
    b bool
)

var c, d int = 1, 2
var e, f = 123, "hello"

//这种不带声明格式的只能在函数体中出现
//g, h := 123, "hello"

func main(){
    g, h := 123, "hello"
    println(x, y, a, b, c, d, e, f, g, h)
}
```