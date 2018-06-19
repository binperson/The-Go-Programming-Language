## for
 - for的条件里不需要括号
 - for的条件里可以省略初始条件，结束条件，递增表达式
 - 省略初始条件，相当于while
 - 无限循环

```go
func main() {

	sum := 0
	for i := 0; i <= 100; i++ {
		sum += i
	}

}
```

```go
func forever()  {
	for {
		fmt.Println("abc")
	}
}
```

```go
func convertToBin(n int) string {
	result := ""
	for ; n > 0; n /= 2 {
		lsb := n % 2
		result = strconv.Itoa(lsb) + result
	}
	return result
}
```

```go
func printFile(filename string)  {
	file, err := os.Open(filename)
	if err != nil {
		panic(err)
	}
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {//类似while go没有while
		fmt.Println(scanner.Text())
	}
}
```

```go
fmt.Println(
		convertToBin(5),// 101
		convertToBin(13),// 1011 -> 1101 打逗号才能换行
	)
	fmt.Println(
		convertToBin(5),// 101
    convertToBin(13))// 打逗号才能换行
```

## 回顾

 - for, if后面的条件没有括号
 - if条件里也可定义变量
 - 没有while
 - switch不需要break，也可以直接switch多个条件