## 数组

 - 数量写在类型前


```go
var arr1 [5]int
arr2 := [3]int{1, 3, 5}
arr3 := [...]int{2, 4, 6, 8, 10}
fmt.Println(arr1, arr2, arr3) // [0 0 0 0 0] [1 3 5] [2 4 6 8 10]

var grid [4][5]int
fmt.Println(grid) // [[0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0]]

```

## 数组的遍历

 - 可通过_省略变量
 - 不仅range，任何地方都可通过_省略变量
 - 如果只要i，可写成for i := range numbers

```go
arr := [3]int{1, 2, 3}
for i := 0; i < len(arr); i++ {
	fmt.Println(arr[i])
}
/*
1
2
3
*/
// ---
arr := [5]int{1, 2, 3, 4, 5}
func printArray(arr [5]int) {
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
printArray(arr)
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
*/

```

## 为什么要用range

 - 意义明确，美观
 - c++：没有类似能力
 - Java/Python：只能for each value，不能同时获取i，v

## 数组是值类型

 - [10]int 和 [20]int是不同类型
 - 调用func f(arr [10]int)会拷贝数组 (大部分语言都是引用传递)
 - 在go语言中一般不直接使用数组

```go
// 切片
func printArry(arr []int) {

}
//数组
func printArry(arr [5]int) {

}


// 做一份拷贝
arr := [5]int{1, 2, 3, 4, 5}
func printArray(arr [5]int) {
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
printArray(arr)

fmt.Println(arr)
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
[1 2 3 4 5]
*/


// 指针

arr := [5]int{1, 2, 3, 4, 5}
func printArray(arr *[5]int) {
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
printArray(&arr)

fmt.Println(arr)
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
*/
```