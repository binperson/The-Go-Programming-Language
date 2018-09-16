## 函数
 - 函数可返回多个值
 - 中断程序需处理
```go
func eval1(a, b int, op string) (int, error) {
	switch op {
	case "+":
		return a + b, nil
	case "-":
		return a - b, nil
	case "*":
		return a * b, nil
	case "/":
		q, _ := div1(a, b)//不想用到就下划线
		return q, nil
	default:
		return 0, fmt.Errorf("unsupported operation:" + op)
	}
}
```

```go
// 13 / 3 = 4 ... 1
func div(a, b int) (int, int) {
	return a / b, a % b
}
```

 - 函数返回多个值可以起名字

```go
func div1(a, b int) (q, r int) {
	return a / b, a % b //建议写具体返回值
}
```

 - 仅用于非常见的函数 return

```go
func div2(a, b int) (q, r int) {
	q = a / b
	r = a % b
	return//仅用于非常间的函数
}
```

 - 对调用者而言没有区别
 - go语言没有默认参数，函数重载

```go
func apply(op func(int, int) int, a, b int) int {
	p := reflect.ValueOf(op).Pointer()
	opName := runtime.FuncForPC(p).Name()
	fmt.Printf("calling function %s with args" + " (%d, %d) ", opName, a, b)
	return op(a, b)
}
func pow(a, b int) int {
	return int(math.Pow(float64(a), float64(b)))
}

fmt.Println(apply(
		func(a int, b int) int {
			return int(math.Pow(float64(a), float64(b)))
		},
		3,4))//calling function main.main.func1 with args (3, 4) 81 第一个main是包名
		
func sum(numbers ...int) int {
	s := 0
	for i := range numbers {
		s += numbers[i]
	}
	return s
}
```

## 回顾

 - 返回值类型写在最后面
 - 可返回多个值
 - 函数作为参数
 - 没有默认参树，可选参数，函数重载，操作符重载
