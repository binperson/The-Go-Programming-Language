## 切片

 - Slice不是值类型，是一个视图
 - Slice本身没有数据，是对底层array的一个view
```go
arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7}
s := arr[2:6]

// s的值为[2 3 4 5]
```

```go
    arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7}

	fmt.Println("arr[2:6] =", arr[2:6]) // arr[2:6] = [2 3 4 5]
	fmt.Println("arr[:6] =", arr[:6]) // arr[:6] = [0 1 2 3 4 5]
	fmt.Println("arr[2:] =", arr[2:]) // arr[2:] = [2 3 4 5 6 7]
	fmt.Println("arr[:] =", arr[:]) // arr[:] = [0 1 2 3 4 5 6 7]
	s1 := arr[2:]
	
	updateSlice(s1)
	fmt.Println(s1) // [100 3 4 5 6 7]
    fmt.Println(arr) // [0 1 100 3 4 5 6 7]
    
    s1 = s1[:5]
```

```go
func printArray(arr []int) {
	arr[0] = 100
	for i := range arr {
		fmt.Println(i)
	}
	for _, v := range arr {
		fmt.Println(v)
	}
	for i, v := range arr {
		fmt.Println(i, v)
	}
}
    arr := [5]int{1, 2, 3, 4, 5}
	printArray(arr[:])
    fmt.Println(arr)
    s2 := arr[:5]
	fmt.Println(s2)
    s2 = s2[2:]
	fmt.Println(s2)
/*
0
1
2
3
4
100
2
3
4
5
0 100
1 2
2 3
3 4
4 5
[100 2 3 4 5]
[100 2 3 4 5]
[3 4 5]

*/
```

### problem

 - s1的值为？
 - s2的值为？

```go
arr := [...]int{1, 2, 3, 4, 5, 6, 7}
s1 := arr[2:6]
s2 := s1[3:5]
fmt.Println(s1) // [3 4 5 6]
fmt.Println(s1[0]) // 3
fmt.Println(s2) // [6 7]
fmt.Printf("s1=%v,len(s1)=%d,cap(s1)=%d\n", s1,len(s1),cap(s1)) // s1=[3 4 5 6],len(s1)=4,cap(s1)=5
```

 - slice可以向后拓展，不可以向前拓展
 - s[i]不可以超越len(s), 向后拓展不可以超越底层数组cap[s]

 ### 向slice添加元素

  - 添加元素时如果超越cap，系统会重新分配更大的底层数组
  - 由于值传递的关系，必须接收append的返回值
  - s = append(s, val)

 ```go
    arr := [...]int{1, 2, 3, 4, 5, 6, 7}
	s1 := arr[2:6]
	fmt.Println(s1) // [3 4 5 6]

	s3 := append(s1, 10)
	s4 := append(s3, 11)
	s5 := append(s4, 12)

	// s4 and s5 no longer view arr.
	fmt.Println("s3, s4, s5 =", s3, s4, s5) //s3, s4, s5 = [3 4 5 6 10] [3 4 5 6 10 11] [3 4 5 6 10 11 12]
    fmt.Println("arr =", arr) //arr = [1 2 3 4 5 6 10]
```

### slice ops

```go
func printSlice(s []int) {
	fmt.Printf("%v, len=%d, cap=%d\n",
		s, len(s), cap(s))
}
    var s []int // Zero value for slice is nil    s == nil

	for i := 0; i < 100; i++ {
		printSlice(s)
		s = append(s, 2*i+1)
	}
/*
[], len=0, cap=0
[1], len=1, cap=1
[1 3], len=2, cap=2
...
*/
    s1 := []int{2, 4, 6, 8}
	printSlice(s1) // [2 4 6 8], len=4, cap=4

	s2 := make([]int, 16)
	s3 := make([]int, 10, 32)
	printSlice(s2) // 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0], len=16, cap=16
	printSlice(s3) // [0 0 0 0 0 0 0 0 0 0], len=10, cap=32

    fmt.Println("Copying slice")
	copy(s2, s1)
	printSlice(s2) // [2 4 6 8 0 0 0 0 0 0 0 0 0 0 0 0], len=16, cap=16

	fmt.Println("Deleting elements from slice")
	s2 = append(s2[:3], s2[4:]...)
	printSlice(s2) // [2 4 6 0 0 0 0 0 0 0 0 0 0 0 0], len=15, cap=16

	fmt.Println("Popping from front")
	front := s2[0]
	s2 = s2[1:]

	fmt.Println(front) // [4 6 0 0 0 0 0 0 0 0 0 0 0 0], len=14, cap=15
	printSlice(s2)

	fmt.Println("Popping from back")
	tail := s2[len(s2)-1]
	s2 = s2[:len(s2)-1]

	fmt.Println(tail) // 0
	printSlice(s2) // [4 6 0 0 0 0 0 0 0 0 0 0 0], len=13, cap=15

```