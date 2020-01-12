> “Go is not meant to innovate programming theory. It’s meant to innovate programming practice.” – Samuel Tesla

# 函数介绍

函数可以说是开发大型软件的基石，也是封装代码的基本单位。在一些比较老的不支持 oop 的编程语言中，正是一个个函数构建起来大型软件。
其实之前的代码中已经简单使用过函数，比如我们针对每一种语法特性都写了一个叫做 `func testXXX` 的函数来在 main
函数里调用看执行结果。

本章来分享一些业务开中常用的 go 函数的语法特性，学会函数之后就可以开始实现逻辑封装了。

# 如何定义一个函数

go 定义一个函数比较简单，语法如下：

```go
// optionalParameters 是 (param1 type1, param2 type2 ...) 这种形式
func functionName(optionalParameters) optionalReturnType {
  body
}
```

来看一个非常简单的函数，计算两个数字之和：

```go
func sum0(a int, b int) int {
	return a + b
}
```

是不是很简单，有个小技巧就是如果多个参数类型一致，可以只写一个类型声明，比如：

```go
func sum1(a, b int) int {
	return a + b
}
```

我们甚至还可以给返回值命名，这个时候需要通过赋值的方式来更新结果，而且 return 可以不用带返回值

```go
func sum2(a, b int) (res int) {
	res = a + b
	return
}
```

go 还支持可变参数，在 python 里我们知道使用的是 `*args`，在 go 里边使用三个省略号来实现，
比如想要计算 n 个 int 数字之和，可以这么写：(注意可变参数其实被包装成了一个 slice)

```go
func sum3(init int, vals ...int) int {
	sum := init
	for _, val := range vals { // vals is []int
		sum += val
	}
	return sum
}
// fmt.Println(sum3(0, 1, 2, 3))
```

再进一步，函数还可以返回多个值，这个相比 c 来说非常方便，比如除了 sum 之外我们再返回一个可变参数的个数：
(其实 go 最后一个参数经常用来返回错误类型，这个之后讨论错误处理的时候再涉及)

```go
func sum4(init int, vals ...int) (int, int) {
	sum := init
	fmt.Println(vals, len(vals))
	for _, val := range vals {
		sum += val
	}
	return sum, len(vals)
}
```

这大概就是函数定义的常见方式，虽然它们形式上很简单，但其实已经包含了很多基本的要素，其他复杂的函数无非是更多的参数，
更加复杂的参数或者返回值类型而已。

# 泛型

先卖个关子，go 目前为止还没有直接提供泛型支持，我们可以使用空接口 interface{} 来实现，之后讲到接口的时候我们再来看如何实现。

# 默认参数

很遗憾，go 开发者老顽固们拒绝支持默认参数，不过倒是有一些比较 trick 的方法来实现。
一种是通过传递零值并且代码里判断是否是零值来实现，另一种是通过传递一个结构体来实现(结构体章节再讲)。

```go
// https://stackoverflow.com/questions/19612449/default-value-in-gos-method
// 可以通过传递零值或者 nil 的方式来判断。
// Both parameters are optional, use empty string for default value
func Concat1(a string, b int) string {
	if a == "" {
		a = "default-a"
	}
	if b == 0 {
		b = 5
	}
	return fmt.Sprintf("%s%d", a, b)
}
```

# 函数的传参

每当学习一门新语言的时候，我都会留意下函数的传值问题，究竟是值传递(copy 参数的值)还是引用传递(传入指针)。
这两种参数传递方式最大的区别就是我们是否可以修改传入参数的值。 先来看一个小例子，尝试传入一个字符串然后修改它，看看是否起作用：

```go
func changeStr(s string) {
	s = "hehe"
	fmt.Println(s)
}
func main() {
	name := "lao wang"
	changeStr(name)
	fmt.Println(name) // 打印出来还是 "lao wang"，没有修改成功，似乎是『值传递』
}
```

看起来似乎是值传递，并没有修改传入的值。好，如果你那么人为，那再试试如果我们传递一个 map 作为参数呢？

```go
func changeMap(m map[string]string) {
	m["王八"] = "绿豆"
}

func main() {
	m := map[string]string{"name": "lao wang"}
	changeMap(m)
	fmt.Println(m) // map[name:lao wang 王八:绿豆], 似乎又可以修改了，函数里的修改起作用了
}
```

其实记住以下这些你就知道什么时候可以修改传入的参数了：

- 内置类型：数值类型、字符串、布尔类型、数组。传递的是副本 (所以一般不用数组啦)
- 引用类型: 切片、映射、通道、接口和函数类型。通过复制传递应用类型值的副本，本质上就是共享底层数据结构

这里其实 map/slice 等也是传递的副本，为啥它们就可以修改呢？我们以 slice
举例，它的内部实现其实是这样的，底层实现包含一个指向数组的指针(ptr), 一个长度 len 和容量 cap
，传参的时候实际上是 slice 这个结构体的拷贝(只有三个元素而不是copy所有的数组值)，所以复制很轻量，而且通过底层的指针就可以实现修改了。

```go
// https://golang.org/src/runtime/slice.go
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

![](./slice-internal.png)

# 传递指针

如果你学过c/c++，你可能会遇到各种费解的指针操作。go 也有指针，但是 go
里边大大简化和限制了指针的使用，所以只要你知道指针的基本概念就可以应付几乎所有场景了。
后文讲结构体的时候，我们在来看下如何传递通过传递结构体指针来修改一个结构体，你会发现大部分指针的使用场景都是针对复杂的结构体。

```go
func changeString(s *string) {
	*s = "new lao wang"
}

func main() {
	s := "lao wang"
	changeString(&s)
	fmt.Println(s)
}
```

# 高阶函数

# 闭包问题

# 递归函数

# 参考：

- [Go Slices: usage and internals](https://blog.golang.org/go-slices-usage-and-internals)