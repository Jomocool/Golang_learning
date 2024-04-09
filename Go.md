# Go

## Go 的优势

1. 极简单的部署方式
   - 可直接编译成机器码
   - 不依赖其他库
   - 直接运行即可部署
2. 静态类型语言
   - 编译的时候检查出来隐藏的大多数问题
3. 语言层面的并发
   - 天生的基因支持
   - 充分的利用多核
4. 强大的标准库
   - runtime系统调度机制
   - 高效的GC垃圾回收
   - 丰富的标准库
5. 简单易学
   - 25个关键字
   - C语言简介基因，内嵌C语法支持
   - 面向对象特征（继承、多态、封装）
   - 跨平台



## main初见

```go
package main // 程序的包名

// import多个包时，用小括号()
import (
	"fmt"
	"time"
)

// main函数
func main() { // 函数的{ 一定要和函数名在同一行
	// golang中的表达式，加';' 和不加都可以，建议不加
	fmt.Println("hello Go!")

	time.Sleep((1 * time.Second))
}
```



## 变量声明

```go
package main

/*
	四种变量的声明方式
*/

import (
	"fmt"
)

// 声明全局变量可以用方法1、方法2、方法3
var gA int
var gB int = 100
var gC = 200

// 方法4只能在函数体内声明

func main() {
	// 1.直接声明，默认值是0
	var a int
	fmt.Println("a = ", a)
	fmt.Printf("type of a is %T\n", a) // 要用printf

	// 2.声明并初始化
	var b int = 100
	fmt.Println("b = ", b)

	var s string = "abcd"
	fmt.Printf("s = %s, type of s is %T\n", s, s)

	// 3.自动类型推导
	var c = 0.0
	fmt.Printf("type of c is %T\n", c)

	// 4.(最常用的方法)省去var关键字
	d := 100
	fmt.Println("d = ", d)
	fmt.Printf("type of d is %T\n", d)

	// 声明多个变量
	// 单行多变量声明
	var xx, yy int = 100, 200
	fmt.Println("xx = ", xx, "yy = ", yy)

	var kk, ll = 100, "Jomo"
	fmt.Println("kk = ", kk, "ll = ", ll)

	// 多行多变量声明
	var (
		vv = 100
		jj = true
	)
	fmt.Println("vv = ", vv, "jj = ", jj)
}
```

## const与iota

```cpp
package main

import "fmt"

// const定义枚举类型
const (
	// iota 只能出现在const()中
	//  可以在const() 添加关键字iota，每行的iota都会累加1，第一行的iota默认值是0
	BEIJING  = 10 * iota // 10*0
	SHANGHAI             // 10*1
	SHENZHEN             // 10*2 (看格式而不是值)
)

// const定义多枚举
const (
	a, b = iota + 1, iota + 2 // iota=0: a=1,b=2
	c, d                      // iota=1: c=2,d=3
	e, f                      // iota=2: e=3,f=4
	// 形式改变
	g, h = iota * 2, iota * 3 // iota=3: g=6,h=9
	i, k                      // iota=4: i=8,k=12
)

func main() {
	// 常量(只读)，不允许修改
	const length int = 10

	fmt.Println("BEIJING = ", BEIJING)
	fmt.Println("SHANGHAI = ", SHANGHAI)
	fmt.Println("SHENZHEN = ", SHENZHEN)
}
```

## 函数多返回值

```cpp
package main

import "fmt"

// 函数定义：func <function_name> (<params:type ...>) <return type> {}
func foo1(a string, b int) int {
	fmt.Println("-----foo1-----")
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)

	c := 100

	return c
}

// 多返回值(匿名)函数
func foo2(a string, b int) (int, int) {
	fmt.Println("-----foo2-----")
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)

	return 666, 777
}

// 多返回值(命名)函数
func foo3(a string, b int) (r1 int, r2 int) {
	fmt.Println("-----foo3-----")
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)

	r1 = 1000
	r2 = 2000

	return
}

// 多返回值(命名)函数，同类型返回值可以省略类型关键字
func foo4(a string, b int) (r1, r2 int) {
	fmt.Println("-----foo4-----")
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)

	r1 = 1000
	r2 = 2000

	return
}

func main() {
	c := foo1("abc", 555)
	fmt.Println("c = ", c)

	ret1, ret2 := foo2("haha", 999)
	fmt.Println("ret1 = ", ret1, "ret2 = ", ret2)

	ret1, ret2 = foo3("foo3", 333)
	fmt.Println("ret1 = ", ret1, "ret2 = ", ret2)

	ret1, ret2 = foo4("foo4", 444)
	fmt.Println("ret1 = ", ret1, "ret2 = ", ret2)
}
```

## import导包路径以及init函数流程

**main.go**

```go
package main

// go env中的GO111MODULE设置为false或auto，否则不会在$GOPATH目录下查找包
// 导入lib1、lib2包，注意与$GOPATH的相对路径
import (
	"GolangStudy/5-init/lib1"
	"GolangStudy/5-init/lib2"
)

func main() {
	lib1.Lib1Test()
	lib2.Lib2Test()
}
```

**lib1/lib1.go**

```go
package lib1

import "fmt"

// lib1提供的API
// Go模块中导出的函数名必须大小！！！
func Lib1Test() {
	fmt.Println("lib1 test...")
}

// lib1的初始化函数，可以做一些初始化操作
func init() {
	fmt.Println("lib1 init...")
}
```

**lib2/lib2.go**

```go
package lib2

import "fmt"

// lib2提供的API
// Go模块中导出的函数名必须大小！！！
func Lib2Test() {
	fmt.Println("lib2 test...")
}

// lib2的初始化函数，可以做一些初始化操作
func init() {
	fmt.Println("lib2 init...")
}
```

> jomo@JomoVM:~/go/src/GolangStudy/5-init$ `go run main.go`
> lib1 init...
> lib2 init...
> lib1 test...
> lib2 test...

## import匿名及别名导包方式

```go
package main

import (
	_ "GolangStudy/5-init/lib1" // 匿名导包，用于想调用lib的init函数，但是不想使用lib接口的情况
	// mylib2 "GolangStudy/5-init/lib2" // 别名导包，lib2改名为mylib2
	. "GolangStudy/5-init/lib2" // 不用包名即可调用接口
)

func main() {
	// mylib2.Lib2Test()
	Lib2Test()
}
```

> jomo@JomoVM:~/go/src/GolangStudy/5-init$ `go run main.go `
> lib1 init...
> lib2 init...
> lib2 test...

## 指针

与C++相同

```go
package main

import . "fmt"

func swap(pa *int, pb *int) (old_a, old_b int) {
	// h
	old_a = *pa
	old_b = *pb

	tmp := *pa
	*pa = *pb
	*pb = tmp

	return
}

func main() {
	a := 100
	b := 200
	old_a, old_b := swap(&a, &b)

	Println("old_a = ", old_a, "old_b = ", old_b)
	Println("a = ", a, "b = ", b)
}
```

> jomo@JomoVM:~/go/src/GolangStudy/6-pointer$ `go run pointer.go`
> old_a =  100 old_b =  200
> a =  200 b =  100

## defer语句调用顺序

`defer <do_something>`定义函数结束退出前的动作，会压入栈中，所以执行顺序和声明顺序相反

`defer`在`return`之后调用

```go
package main

import "fmt"

func deferCall1() {
	fmt.Println("defer call 1...")
}

func deferCall2() {
	fmt.Println("defer call 2...")
}

func returnCall() int {
	fmt.Println("return call...")
	return 0
}

func deferandreturn() int {
	// 由于returnCall()属于在当前函数周期内执行的，因此只有当returnCall执行完毕后才会去执行deferCall
	defer deferCall1()
	defer deferCall2()
	return returnCall()
	// deferCall2() 在这里执行。因为只有return完之后，函数调用才算结束了
	// deferCall1()
}

func main() {
	deferandreturn()
}
```

> jomo@JomoVM:~/go/src/GolangStudy/7-defer$ `go run defer.go`
> return call...
> defer call 2...
> defer call 1...



## 数组与动态数组

**array.go**

```go
package main

import "fmt"

func main() {
	// 定义一个固定长度的数组
	var myArray1 [10]int
	myArray2 := [10]int{1, 2, 3, 4}
	myArray3 := [4]int{1, 2, 3, 4}

	for i := 0; i < len(myArray1); i++ {
		fmt.Print(myArray1[i], " ")
		if i == len(myArray1)-1 {
			fmt.Println("")
		}
	}

	for index, value := range myArray2 {
		fmt.Println("index = ", index, ", value = ", value)
	}

	fmt.Printf("myArray1 type = %T\n", myArray1)
	fmt.Printf("myArray2 type = %T\n", myArray2)
	fmt.Printf("myArray3 type = %T\n", myArray3)
}
```

**slice.go**

```go
package main

import "fmt"

func printArray(myArray []int) {
	for _, value := range myArray {
		fmt.Println("value = ", value)
	}

	myArray[0] = 100
}

func main() {
	// 动态数组，切片slice
	myArray := []int{1, 2, 3, 4}

	fmt.Printf("myArray type = %T\n", myArray)

	// slice是引用传递
	printArray(myArray)

	fmt.Println("===========================")

	// 查看myArray[0]是否修改成功
	for _, value := range myArray {
		fmt.Println("value = ", value)
	}
}
```

