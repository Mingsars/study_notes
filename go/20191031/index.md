# 1. 通过函数修改结构体属性
### 1. &emsp;js惯用手法
```go
package main

import "fmt"

type Books struct {
    title string
    author string
    book_id int
}

func changeTitle(book Books) {
    book.title = "changeTitle"
}

func main() {
    book1 := Books{"book1", "mingshan", 23}

    changeTitle(book1)

    fmt.Printf("book: %v\n", book1)
}
```
#### &emsp;&emsp;打印结果为
```go
{boo1 mingshan 23}
```
#### &emsp;&emsp;发现 book1 的title属性值并没有被改变，因为修改的只是参数的title属性，而没有修改本体的属性
#### &emsp;&emsp;为了修改本体属性，需要传入指针

### 2. &emsp;Go正确姿势
```go
package main

import "fmt"

type Books struct {
    title string
    author string
    book_id int
}

func changeTitle(book *Books) {
    book.title = "changeTitle"
}

func main() {
    book1 := Books{"book1", "mingshan", 23}

    changeTitle(&book1)

    fmt.Printf("book: %v\n", book1)
}
```
#### &emsp;&emsp;打印结果为
```go
{changeTitle mingshan 23}
```
# 2. 语言切片（slice）
#### &emsp;&emsp;和数组不同的是，切片长度可改变，长度不固定
### 1. &emsp;切片初始化
```go
var sli  = [] int {1, 2, 3}

// 或者
sli := [] int {1, 2, 3}

// 或者
var sli = make([] int, 3, 3)
```
### 2. &emsp;切片截取
```go
arr := [] int {1, 2, 3, 4}

sli := arr[1:3] //  [2, 3]
```
#### &emsp;&emsp; 切片截取中，：左边表示从下标为 1 的地方开始截取，截取到下标为 3 的前一个值
### 3. &emsp;空切片（nil）
#### &emsp;&emsp;一个切片在未初始化之前默认为nil，注意不是null，长度为0
```go
package main

import "fmt"

func main() {
   var numbers []int

   printSlice(numbers)

   if(numbers == nil){
      fmt.Printf("切片是空的")
   }
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
#### &emsp;&emsp;上面代码输出结果为：
```go
len=0 cap=0 slice=[]
切片是空的
```
### 3. &emsp;append() 和 copy() 函数
```go
package main

import "fmt"

func main() {
    s1 := [] int {1, 2, 3}
    s2 := mark([] int, len(s1), (cap(s1) * 2))  // len()函数获取切片长度， cap()函数获取切片最大容量
    
    fmt.Println(s2) //  [0, 0, 0]

    copy(s2, s1)
    fmt.Println(s1) //  [1, 2, 3]
    fmt.Println(s2) //  [1, 2, 3]

    fmt.Println(append(s1, 4, 5, 6))    //  [1, 2, 3, 4, 5, 6]
    fmt.Println(s1) //  [1, 2, 3]

    s1 = append(s1, 7, 8, 9)
    fmt.Println(s1) //  [1, 2, 3, 7, 8, 9]
    fmt.Println(s2) //  [1, 2, 3]
}
```
#### &emsp;&emsp;从上面代码看得出来，append()函数并不会直接改变原切片，copy()函数也可以理解为js中的深拷贝
# 2. 语言范围（range）
#### &emsp;&emsp;Go 语言中 range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 key-value 对。
## &emsp;&emsp;为了快捷，从这里开始省略包名，和fmt包引入，需另外引入则写入
```go
func main() {
    //  数组/切片
    nums := []int {1, 2, 3, 4}
    sum := 0

    for i, val := range nums {
        sum += val
        fmt.Printf("%d: %d", i, val)
    }
    fmt.Println(sum)
    //  0: 1
    //  1: 2
    //  2: 3
    //  3: 4
    //  10

    //  map（js中的对象）
    kvs := map[string]string {"a": "new", "b": "old"}

    for k, v := range kvs {
        fmt.Printf("%s: %s", k, v)
    }
    //  a: new
    //  b: old

    //  字符串
    for i, c := range "go" {
        fmt.Println(i, c)
    }
    //  0: 103
    //  1: 111
}
```