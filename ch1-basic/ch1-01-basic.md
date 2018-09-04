## 变量定义

##### 使用var关键字
 
 - var a, b, c bool
 - var s1, s2 string = "hello", "world"
 - 可以放在函数内，或直接放在包内
 - 使用var()集中定义变量
 - 变量名在变量类型前

[golang fmt格式“占位符”](https://studygolang.com/articles/2644)

 ```go
func variableZeroValue() {
	var a int
	var s string
	fmt.Printf("%d %q\n", a, s) // 0 ""
}
```

```go
func variableInitialValue() {
        // 变量赋初值
	var a, b int = 3, 4
	var s string = "abc"
	fmt.Println(a, b, s) // 3 4 abc
}
```

```go
package main

import "fmt"

//函数外层必须有var和func关键字  不能:=
var ss = 3
var (
	aa = "kkk"
	bb = true
)
func main() {
	fmt.Println(ss, aa, bb) // 3 kkk true
}

```

##### 让编译器自动决定类型

 - var a, b, i, s1, s2 = true, false, 3, "hello", "world"

```go
func variableTypeDeduction()  {
	var a, b, c, s = 3, 4, true, "def"
	fmt.Println(a, b, c, s) //3 4 true "def"
}
```

##### 使用:=定义变量
 - a, b, i, s1, s2 := true, false, 3, "hello", "world"
 - 只能在函数内使用

```go
func variableShorter()  {
	a, b, c, s := 3, 4, true, "def"
	b = 5
	fmt.Println(a, b, c, s) //3 5 true "def"
}
```

## 内建变量类型
 - bool string
 - (u)int, (u)int8, (u)int16, (u)int32, (u)int64, uintptr(指针)  加u就是无符号整数，不加就是无符号整数
 - byte, rune(字符类型 32位)
 - float32, float64, complex64, complex128 (complex复数 complex64实部和虚部分别是32位， complex128实部和虚部分别是64位)

 ## 复数回顾
 - i = 根号-1
 - 复数：3 + 4i
 - |3 + 4i| = 5

 ```go
func euler() {
	fmt.Printf("%.3f\n",
		cmplx.Exp(1i*math.Pi)+1) // (0.000+0.000i)
}
 ```

 ## 强制类型转换
 - 类型转换是强制的
 - var a,b int = 3, 4
 - var c int = math.Sqrt(a*a + b*b) 坑
 - var c int = int(math.Sqrt(float64(a*a + b*b))) //float转出来有问题

 ```go
package main

import (
	"fmt"
	"math"
)

func triangle() {
	var a, b int = 3, 4
	fmt.Println(calcTriangle(a, b))
}

func calcTriangle(a, b int) int {
	var c int
	c = int(math.Sqrt(float64(a*a + b*b)))
	return c
}
func main() {
	triangle() //5
}

 ```

 ## 常量定义
 - const filename = "abc.txt"
 - const 数值可作为各种类型使用
 - const a, b = 3,4
 - var c int = int(math.Sqrt(a*a + b*b))

 ```go
 func consts() {
	const filename string = "abc.txt"
	const (
		a, b = 3, 4
	)
	var c = math.Sqrt(a*a + b*b)
	fmt.Println(c)//5
	var d int = int(math.Sqrt(a*a + b*b))
	fmt.Println(d)//5
}
```

## 使用常量定义枚举类型
 - 普通枚举类型
 - 自增值枚举类型

 ```go
 func enums1() {
	const(
		cpp = iota
		java
		python
		golang
	)
	fmt.Println(cpp, java, python, golang)//0 1 2 3
}
func enums2() {
	const(
		cpp = iota
		_
		python
		golang
	)
	// b, kb, mb, gb, tb, pb
	const (
		b = 1 << (10 * iota)
		kb
		mb
		gb
		tb
		pb
	)
	fmt.Println(cpp, python, golang)//0 2 3
	fmt.Println(b, kb, mb, gb, tb, pb)//1 1024 1048576 1073741824 1099511627776 1125899906842624
}
```

## 回顾
 - 变量类型写在变量名之后
 - 编译器可推测变量类型
 - 没有char， 只有rune
 - 原生支持复数类型
