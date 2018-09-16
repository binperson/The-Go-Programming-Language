## 指针
 - 指针不能运算
 - go语言只有值传递一种方式

```go
var a int = 2
var pa *int = &a
*pa = 3
fmt.Println(a) // 3
```

## 参数传递

 - go语言只有值传递一种方式(函数运行一直，值就拷贝一次，但是我们有指针)  cache

```go

func swap(a, b int)  {
	a, b = b, a
}
func swap1(a, b *int)  {
	*a, *b = *b, *a
}

fmt.Println(sum(1, 2, 3))//6
a, b := 3, 4
swap(a, b)
fmt.Println(a, b)//3, 4
swap1(&a, &b)
fmt.Println(a, b)//4, 3
```