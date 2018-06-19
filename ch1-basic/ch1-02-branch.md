## if

 - if的条件里可以赋值
 - if的条件里赋值的变量作用域就在这个if语句里

```go
const filename = "abc.txt"
contents, err := ioutil.ReadFile(filename)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Printf("%s\n", contents) // binperson
}
// 
func test() {
	const filename = "abc.txt"
	if contents, err := ioutil.ReadFile(filename); err !=nil {
		fmt.Println(err)
	} else {
		fmt.Printf("%s\n", contents) // binperson
	}
}
```

## switch

 - switch会自动break，除非使用fallthrough
 - switch后可以没有表达式

```go
func eval(a, b int, op string) int {
	var result int
	switch op {
	case "+":
		result = a + b
	case "-":
		result = a - b
	case "*":
		result = a * b
	case "/":
		result = a / b
	default:
		panic("unsupported operator" + op)
	}
	return result
}

func grade(score int) string {
	g := ""
	switch {
	case score < 0 || score > 100:
		panic(fmt.Sprintf("Wrong score: %d", score))//panic会中断程序执行
	case score < 60:
		g = "F"
	case score < 80:
		g = "C"
	case score < 90:
		g = "B"
	case score <= 100:
		g = "A"
	}
	return g
}
 ```