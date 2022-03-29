#### `声明`

##### `var`

##### `type`

##### `const`

##### `func`

#### `new` 函数





# **基础**

## 包

每个 Go 程序都是由包构成的。
程序从 `main` 包开始运行
包名与导入路径的最后一个元素一致，使用 包名.函数的形式进行调用。

`go package的两个约束：`

1、同一个目录下的go文件，如果package是main,那么这个目录下的go文件只能有一个main函数。

2、同一个目录下的go文件，只能是一个package。 （注意： 这里说的是同一个目录下的go文件，但不是同一个目录下的其他目录中的go文件）

`package的作用：`

　　1、同一个package下可以共享**变量**

　　2、同一个package下可以共享**函数**

~~~go
package main

//使用圆括号组合导入包
import (
	"fmt"
	"math/rand"
    //包名重叠，重新导包
    "math"
)

func main() {
    //在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的
    //我们只能引用已导出的名字，Println、Intn
    //任何未导出的包名在包外均无法访问
	fmt.Println("My favorite number is", rand.Intn(10))
    fmt.Println(math.Pi)
}

~~~

#### go get引入第三方package

1. 将git上的库下载到本地，这条命令默认下载到 $GOPATH下的/src/下,并与package的目录结构一致

```GO
go get github.com/icexin/golib
```

2. 在/src/的其他目录下新创建一个目录如：/thirdlib/，作为另一个package，然后在目录下新建一个main.go文件，并导入刚才下载的第三方package![img](773921-20170616154438181-1205394341.png)

   这里引入的第三方package 是一个全路径，这个路径就是从 $GOPATH/src/开始算起。所以，要写的路径就是从**/src/开始**

## 变量、函数

1. 注意类型在变量名 **之后**
   Go语法变量名在前,类型在后,从左至右,解释型语法
   https://blog.go-zh.org/gos-declaration-syntax

2. `var` 语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后。
   `var` 语句可以出现在**包或函数级别**
3. 短变量声明
   在函数中，简洁赋值语句 `:=` 可在类型明确的地方代替 `var` 声明
   函数外的每个语句都必须以关键字开始（`var`, `func` 等等），因此 `:=` 结构**不能在函数外使用**。



```go
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

//包级别变量
var c, python, java bool

//变量声明可以包含初始值，每个变量对应一个。
var i,j int = 1, 2

//如果初始化值已存在，则可以省略类型
var c, python, java = true, false, "no!"

//短变量声明
k := 3
fruit, isFruit := "Aplle", true

//当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略。
func addTwo(x,y int) int {
	return x + y;
}

//函数可以返回任意数量的返回值,此处返回了两个字符串
func swap(x, y string) (string, string) {
	return y, x
}

//Go 的返回值可被命名，它们会被视作定义在函数顶部的变量
//没有参数的 return 语句返回已命名的返回值,影响可读性
func split(sum int) (x,y int) {
    x = sum + 1
    y = sum - 1
    return
}

func main() {
    //函数级别变量
    var i,j int
	fmt.Println(add(42, 13))
    a, b :=swap("hello", "world")
    fmt.Println(a, b)
}

```

## 基本类型

1. 没有明确初始值的变量声明会被赋予它们的 **零值**。

- 数值类型为 `0`，
- 布尔类型为 `false`，
- 字符串为 `""`（空字符串）

用类型转换或浮点数语法来声明并初始化一个浮点数值：

```go
z := 1.0
z := float64(1)
```



```go

//基本类型
//当你需要一个整数值时应使用 int 类型，除非你有特殊的理由使用固定大小或无符号的整数类型。
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
    // 表示一个 Unicode 码点

float32 float64

complex64 complex128
```

```go
package main

import (
	"fmt"
	"math/cmplx"
)

//同导入语句一样，变量声明也可以“分组”成一个语法块。
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)
	fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)
	fmt.Printf("Type: %T Value: %v\n", z, z)
}
```

## 类型转换

Go 在不同类型的项之间赋值时需要显式转换

```go
数值转换1
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

更简洁方式
i := 42
f := float64(i)
u := uint(f)

```

## 类型推导

在声明一个变量而不指定其类型时（即使用不带类型的 `:=` 语法或 `var =` 表达式语法），变量的类型由右值推导得出。

```go
var i int
j := i // j 也是一个 int

i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

## 常量

- 常量的声明与变量类似，只不过是使用 `const` 关键字。
- 常量可以是字符、字符串、布尔值或数值。
- 常量不能用 `:=` 语法声明。

```go
const Pi = 3.14
```

## 数值常量

- 数值常量是高精度的 **值**。
- 一个未指定类型的常量由上下文来决定其类型。
- `int` 可以存放最大64位的整数，根据平台不同有时会更少。

~~~go
package main

import "fmt"

const (
	// 将 1 左移 100 位来创建一个非常大的数字
	// 即这个数的二进制是 1 后面跟着 100 个 0
	Big = 1 << 100
	// 再往右移 99 位，即 Small = 1 << 1，或者说 Small = 2
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
    //float64  
	fmt.Println(needFloat(Big))
}

//output
21
0.2
1.2676506002282295e+29
~~~

## 流程控制语句

### for/while

基本形式
一个求平方根的算法（x为开方数，z为猜测的平方值，循环次数愈多愈接近真实值） ：**z  -=  (z * z - x) / (2 * z)**

```go
	sum := 0
	for i := 0; i < 100; i++ {
	sum += i
	}

//for初始化语句和后置语句是可选的。
    sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)

    //while
    sum := 1
	for sum < 1000 {
		sum += sum
	}

	//省略循环条件,为无限循环
     for {
	}
```



### if/else

1. Go 的 if 语句与 for 循环类似，表达式外无需小括号 ( ) ，而大括号 { } 则是必须的。
2. `if` 语句可以在条件表达式前执行一个**简短语句**
3. if 语句的变量作用域只在 if/else 之内

```go
func pow(x, n, lim float64) float64 {
     if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	}
```

### switch

1. Go 在每个 case 后面自动提供`break` 。 除非以 `fallthrough` 语句结束，否则分支会自动终止。 
2. switch 的 case 语句从上到下顺次执行，直到匹配成功时停止

3. Go 的另一点重要的不同在于 switch 的 case 无需为常量，且取值不必为整数

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}

```

4. **没有条件的 switch**
   同 `switch true` 一样。

   这种形式能将一长串 if-then-else 写得更加清晰。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}

```



### defer

- defer 语句会将函数推迟到外层函数返回之后执行。
  推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}

//output
hello world
```

- 推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟的函数会按照后进先出的顺序调用。

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}

//output
done
9
8
7
...
```

## 指针

Go 拥有指针。指针保存了值的内存地址。

类型 `*T` 是指向 `T` 类型值的指针。其零值为 `nil`。

```
var p *int
```

`&` 操作符会生成一个指向其操作数的指针。

```
i := 42
p = &i
```

`*` 操作符表示指针指向的底层值。

```
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```

这也就是通常所说的“间接引用”或“重定向”。

与 C 不同，Go 没有指针运算。

```go
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // 指向 i
	fmt.Println(*p) // 通过指针读取 i 的值
	*p = 21         // 通过指针设置 i 的值
	fmt.Println(i)  // 查看 i 的值

	p = &j         // 指向 j
	*p = *p / 37   // 通过指针对 j 进行除法运算
	fmt.Println(j) // 查看 j 的值
}

```

## 结构体

1. 一个结构体（`struct`）就是一组字段（field）。
2. 结构体字段使用点号来访问。
3. 结构体字段也可以通过结构体指针来访问。
4. 特殊的前缀 `&` 返回一个指向结构体的指针。

```go
package main

import "fmt"

//定义一个结构体
type Vertex struct {
	X int
	Y int
}

func main() {
    //初始化结构体并赋值
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
    
    //一个指向结构体的指针 p
    p := &v
    //隐式间接引用指针 可显示(*p).Y
    fmt.Println(p.Y)
    // 创建一个 *Vertex 类型的结构体（指针）
    p  = &Vertex{1, 2}
}
```

## 数组

类型 `[n]T` 表示拥有 `n` 个 `T` 类型的值的数组。

```go
var a [10]int
primes := [6]int{2, 3, 5, 7, 11, 13}
```

数组的长度是其类型的一部分，因此数组不能改变大小, 可通过`切片`改变

## 切片

每个数组的大小都是固定的。而切片则为数组元素提供动态大小的、灵活的视角。在实践中，切片比数组更常用。

类型 `[]T` 表示一个元素类型为 `T` 的切片。

切片通过两个下标来界定，即一个上界和一个下界，二者以冒号分隔：

```go
a[low : high]
```

它会选择一个半开区间，包括第一个元素，但排除最后一个元素。

以下表达式创建了一个切片，它包含 `a` 中下标从 1 到 3 的元素：

```go
a[1:4]
```

切片并不存储任何数据，它只是描述了底层数组中的一段。

**更改切片的元素会修改其底层数组中对应的元素。**

与它共享底层数组的切片都会观测到这些修改。

切片拥有 **长度** 和 **容量**。

切片的长度就是它所包含的元素个数。

切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

切片 `s` 的长度和容量可通过表达式 `len(s)` 和 `cap(s)` 来获取。

切片的零值是 `nil`。

nil 切片的长度和容量为 0 且没有底层数组。

切片可以用内建函数 `make` 来创建，这也是你创建动态数组的方式。

`make` 函数会分配一个元素为零值的数组并返回一个引用了它的切片：

```go
a := make([]int, 5)  // len(a)=5
```

要指定它的容量，需向 `make` 传入第三个参数：

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

## 向切片追加元素

为切片追加新的元素是种常用的操作，为此 Go 提供了内建的 `append` 函数。内建函数的[文档](https://go-zh.org/pkg/builtin/#append)对此函数有详细的介绍。

```
func append(s []T, vs ...T) []T
```

`append` 的第一个参数 `s` 是一个元素类型为 `T` 的切片，其余类型为 `T` 的值将会追加到该切片的末尾。

`append` 的结果是一个包含原切片所有元素加上新添加元素的切片。

当 `s` 的底层数组太小，不足以容纳所有给定的值时，它就会分配一个更大的数组。返回的切片会指向这个新分配的数组。向切片追加元素



```go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// 添加一个空切片
	s = append(s, 0)
	printSlice(s)

	// 这个切片会按需增长
	s = append(s, 1)
	printSlice(s)

	// 可以一次性添加多个元素
	s = append(s, 2)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

```

### [Go 切片：用法和本质](https://blog.go-zh.org/go-slices-usage-and-internals)

## Range

`for` 循环的 `range` 形式可遍历切片或映射。

当使用 `for` 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

可以将下标或值赋予 `_` 来忽略它。

```
for i, _ := range pow
for _, value := range pow
```

若你只需要索引，忽略第二个变量即可。

```
for i := range pow
```

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}

```

## 映射

映射将键映射到值。

映射的零值为 `nil` 。`nil` 映射既没有键，也不能添加键。

`make` 函数会返回给定类型的映射，并将其初始化备用。

```go
package main

import "fmt"

type Vertex struct {
	Lat,float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

func main() {
	fmt.Println(m)
}

```

## 修改映射

在映射 `m` 中插入或修改元素：

```
m[key] = elem
```

获取元素：

```
elem = m[key]
```

删除元素：

```
delete(m, key)
```

通过双赋值检测某个键是否存在：

```
elem, ok = m[key]
```

若 `key` 在 `m` 中，`ok` 为 `true` ；否则，`ok` 为 `false`。

若 `key` 不在映射中，那么 `elem` 是该映射元素类型的零值。

同样的，当从映射中读取某个不存在的键时，结果是映射的元素类型的零值。

**注** ：若 `elem` 或 `ok` 还未声明，你可以使用短变量声明：

```
elem, ok := m[key]
```

## 函数值

函数也是值。它们可以像其它值一样传递。

函数值可以用作函数的参数或返回值。

```go
	package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}

```

## 函数的闭包

Go 函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，换句话说，该函数被这些变量“绑定”在一起。

例如，函数 `adder` 返回一个闭包。每个闭包都被绑定在其各自的 `sum` 变量上。

```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}

```

# **函数接口**

## 方法

方法只是个带**接收者参数**的函数。

`指针接收者`

```go
type Vertex struct {
    X, Y float64
}

func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}
```

带指针参数的函数必须接受一个指针：

```go
var v Vertex
ScaleFunc(v, 5)  // 编译错误！
ScaleFunc(&v, 5) // OK
```

而以指针为接收者的方法被调用时，接收者既能为值又能为指针：

```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

# 并发

### **`通道死锁`**

使用无缓存通道时，如果主程序的通道操作(读取或写入)在协程之前，则会造成主程序阻塞，协程的通道操作也不能开始。

如果主程序的通道操作(读取或写入)在协程之后，则不会

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	st := time.Now()      // 返回当前的时间戳
	ch := make(chan bool) // 这里make了一个无缓存的channel
	ch <- true
	go func() {
		time.Sleep(time.Second * 2)
		<-ch

	}()

	// 无缓冲，发送方阻塞直到接收方接收到数据。
	fmt.Printf("cost %.1f s\n", time.Now().Sub(st).Seconds()) //计算从st声明到现在的时间
	time.Sleep(time.Second * 5)
}
```

通道的一个循环判断循环 

`for i := range c` 会不断从信道接收值，直到它被关闭。

*注意：* 只有发送者才能关闭信道，而接收者不能。向一个已经关闭的信道发送数据会引发程序恐慌（panic）。

*还要注意：* 信道与文件不同，通常情况下无需关闭它们。只有在必须告诉接收者不再有需要发送的值时才有必要关闭，例如终止一个 `range` 循环。

```go
package main

import "fmt"

func main() {
	ch := make(chan int,3)
	ch <- 1
	ch <- 2
	close(ch)
	for v, ok := <-ch; ok == true; v, ok = <-ch{
		fmt.Println(v, ok)
	}
}

//或者
func main() {
	ch := make(chan int,3)
	ch <- 1
	ch <- 2
	close(ch)
	for i := range ch {
		fmt.Println(i)
	}
}
```

### `Sleep & Tick & After`

golang 写循环执行的定时任务，常见的有以下三种实现方式:
1、time.Sleep方法：

```go
for {
   time.Sleep(time.Second)
   fmt.Println("我在定时执行任务")
}
```



2、time.Tick函数：

```go
t1:=time.Tick(3*time.Second)
for {
   select {
   case <-t1:
      fmt.Println("t1定时器")
   }
}
```

3、其中Tick定时任务，也可以先使用time.Ticker函数获取Ticker结构体，然后进行阻塞监听信息，这种方式可以手动选择停止定时任务，在停止任务时，减少对内存的浪费。

```go
t:=time.NewTicker(time.Second)
for {
   select {
   case <-t.C:
      fmt.Println("t1定时器")
      t.Stop()
   }
}
```

其中第二种和第三种可以归为同一类

**sleep**

```go
// Sleep pauses the current goroutine for at least the duration d.
```

### **`defer`**

Go 语言的 `defer` 会在当前函数返回前执行传入的函数，它会经常被用于关闭文件描述符、关闭数据库连接以及解锁资源。

https://draveness.me/golang/docs/part2-foundation/ch05-keyword/golang-defer/

go **defer** 关键字标记的函数会在当前函数或方法调用结束时执行，当有多个defer时，会将执行结果按照栈的方式存储，且derfer是值引用函数 会预计算参数值(如打印函数执行时间)，可以将函数放入匿名函数中，再用defer 标记

```go
func main() {
	startedAt := time.Now()
	defer func() { fmt.Println(time.Since(startedAt)) }()
	
	time.Sleep(time.Second)
}

$ go run main.go
1s	
```



# RPC

Go语言的RPC规则：方法只能有两个可序列化的参数，其中第二个参数是指针类型，并且返回一个error类型，同时必须是公开的方法。

在涉及RPC的应用中，作为开发人员一般至少有三种角色：首先是服务端实现RPC方法的开发人员，其次是客户端调用RPC方法的人员，最后也是最重要的是制定服务端和客户端RPC接口规范的设计人员。

## `简单RPC`

server

```go
package main

import (
	"fmt"
	"log"
	"net"
	"net/rpc"
)

type HelloService struct{}

func (p *HelloService) Hello(request string, reply *string) error {
	*reply = "hello" + request
	return nil
}
func main() {
	//为HelloService类型的对象注册RPC服务
	//rpc.Register函数调用会将对象类型中所有满足RPC规则的对象方法注册为RPC函数
	//所有注册的方法会放在“HelloService”服务空间之下
	rpc.RegisterName("HelloService", new(HelloService))

	//建立一个唯一的TCP链接
	listener, err := net.Listen("tcp", ":1234")
	if err != nil {
		log.Fatal("ListenTCP error:", err)
	}

	fmt.Println("waiting client to connect ...")

	//等待来自客户端的连接
	//Accept waits for and returns the next connection to the listener.
	conn, err := listener.Accept()
	if err != nil {
		log.Fatal("Accept error", err)
	}

	//通过rpc.ServeConn函数在该TCP链接上为对方提供RPC服务。
	rpc.ServeConn(conn)
}

```

client

```go
package main

import (
	"fmt"
	"log"
	"net/rpc"
)

func main() {
	//首先是通过rpc.Dial拨号RPC服务
	client, err := rpc.Dial("tcp", "localhost:1234")
	if err != nil {
		log.Fatal("dialing", err)
	}

	var reply string
	//然后通过client.Call调用具体的RPC方法
	//第一个参数是用点号链接的RPC服务名字和方法名字
	//后面的第二和第三个参数分别我们定义RPC方法的两个参数
	err = client.Call("HelloService.Hello", "hello", &reply)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(reply)

}

```

RPC的应用中，作为开发人员一般至少有三种角色：

- 首先是服务端实现RPC方法的**开发人员**
- 其次是客户端**调用**RPC方法的**人员**
- 最重要的是制定服务端和客户端RPC**接口规范的设计人员**

## `重构RPC`

1. 第一步需要明确服务的名字和接口

   RPC服务的接口规范分为三个部分:

   - 服务的名字
   - 服务要实现的详细方法列表
   - 注册该类型服务的函数

   ```go
   //为了避免名字冲突，我们在RPC服务的名字中增加了包路径前缀（这个是RPC服务抽象的包路径，并非完全等价Go语言的包路径）
   const HelloServiceName = "path/to/pkg.HelloService"
   
   type HelloServiceInterface = interface {
       Hello(request string, reply *string) error
   }
   
   func RegisterHelloService(svc HelloServiceInterface) error {
       return rpc.RegisterName(HelloServiceName, svc)
   }
   ```

   完整代码

   ```go
   package main
   
   import (
   	"fmt"
   	"log"
   	"net"
   	"net/rpc"
   )
   
   const HelloServiceName = "rpc1.HelloService"
   
   type HelloServiceInterface = interface {
   	Hello(request string, reply *string) error
   }
   
   func RegisterHelloService(svc HelloServiceInterface) error {
   	return rpc.RegisterName(HelloServiceName, svc)
   }
   
   type HelloService struct{}
   
   func (p *HelloService) Hello(requst string, reply *string) error {
   	*reply = "hello:" + requst
   	return nil
   }
   
   func Hello(request string, reply *string) error {
   	*reply = "hello" + request
   	return nil
   }
   func main() {
   	RegisterHelloService(new(HelloService))
   
   	listener, err := net.Listen("tcp", ":1234")
   	if err != nil {
   		log.Fatal("ListenTCP error:", err)
   	}
   
   	for {
   		fmt.Println("waiting client to connect ...")
   		conn, err := listener.Accept()
   		if err != nil {
   			log.Fatal("Accept error:", err)
   		}
   		fmt.Println("处理请求中....")
   		go rpc.ServeConn(conn)
   	}
   }
   
   ```

   

   2.客户端根据规范编写RPC调用代码

   ```go
   func main() {
       client, err := rpc.Dial("tcp", "localhost:1234")
       if err != nil {
           log.Fatal("dialing:", err)
       }
       
       var reply string
       //client变化的地方
       err = client.Call(HelloServiceName + ".Hello", "hello", &reply)
       if err != nil {
           log.Fatal(err)
       }
   }
   ```

   简单包装后的完整代码

   ```go
   package main
   
   import (
   	"fmt"
   	"log"
   	"net/rpc"
   )
   
   type HelloServiceClient struct {
   	*rpc.Client
   }
   
   //在接口规范中针对客户端新增加了HelloServiceClient类型
   //该类型也必须满足HelloServiceInterface接口
   //这样客户端用户就可以直接通过接口对应的方法调用RPC函数
   //??????
   //var _ HelloServiceInterface = (*HelloServiceClient)(nil)
   
   func DialHelloService(network, address string) (*HelloServiceClient, error) {
   	c, err := rpc.Dial(network, address)
   	if err != nil {
   		return nil, err
   	}
   	return &HelloServiceClient{Client: c}, nil
   }
   
   func (p *HelloServiceClient) Hello(request string, reply *string) error {
   	return p.Client.Call("rpc1.HelloService"+".Hello", request, &reply)
   }
   func main() {
   	//在接口规范部分增加对客户端的简单包装
   	//简化客户端用户调用RPC函数
   	client, err := DialHelloService("tcp", "localhost:1234")
   	if err != nil {
   		log.Fatal("dialing:", err)
   	}
   
   	var reply string
   	err = client.Hello("shengyi", &reply)
   	if err != nil {
   		log.Fatal(err)
   	}
   	fmt.Println(reply)
   	////首先是通过rpc.Dial拨号RPC服务
   	//client, err := rpc.Dial("tcp", "localhost:1234")
   	//if err != nil {
   	//	log.Fatal("dialing", err)
   	//}
   	//
   	//var reply string
   	////然后通过client.Call调用具体的RPC方法
   	////第一个参数是用点号链接的RPC服务名字和方法名字
   	////后面的第二和第三个参数分别我们定义RPC方法的两个参数
   	//err = client.Call("HelloService.Hello", "hello", &reply)
   	//if err != nil {
   	//	log.Fatal(err)
   	//}
   	//
   	//fmt.Println(reply)
   
   }
   
   ```

   

## `跨语言RPC(待续)`

## `Http的RPC`(待续)

# IPC

- 发送RPC前、调用需要锁的函数前。先释放锁，除非使用一个新的协程调用

- break前、retuern前 必须先释放锁

  

# IO

```go
ofile, _ := os.Create(oname)
```

判断文件、文件夹是否存在

```go
			if _, err := os.Stat(midFile); err == nil {
				file, _ := os.OpenFile(midFile, os.O_WRONLY|os.O_APPEND|os.O_CREATE, 0666)
			} else if errors.Is(err, os.ErrNotExist) {
				// path/to/whatever does *not* exist
			} else {
				// Schrodinger: file may or may not exist. See err for details.
				// Therefore, do *NOT* use !os.IsNotExist(err) to test for file existence
			}
```

map

> This variable m is a map of string keys to int values:
> `var m map[string]int`
> Map types are reference types, like pointers or slices, and so the value of m above is nil; it doesn't point to an initialized map. A nil map behaves like an empty map when reading, but attempts to write to a nil map will cause a runtime panic; don't do that. To initialize a map, use the built in make function:
> `m = make(map[string]int)`

同为引用类型的slice，在使用`append` 向nil slice追加新元素就可以，原因是`append`方法在底层为slice重新分配了相关数组让nil slice指向了具体的内存地址

> nil map doesn't point to an initialized map. Assigning value won't reallocate point address.
> The append function appends the elements x to the end of the slice s, If the backing array of s is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

# Time 包

   
