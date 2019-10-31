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
