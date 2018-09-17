### 面向对象

 - go语言仅支持封装，不支持继承和多态
 - go语言没有class，只有struct

```go
type TreeNode struct {
	left, right *TreeNode
	Value int
}
```
### 结构的创建

 - 不论地址还是结构本身， 一律使用.来访问成员

```go
package main

import "fmt"

type treeNode struct {
	value       int
	left, right *treeNode
}


func main() {
	var root treeNode
	root = treeNode{value: 3}
	root.left = &treeNode{}
	root.right = &treeNode{5, nil, nil}
	root.right.left = new(treeNode) //c++ root.Right->left

	nodes := [] treeNode {
		{value: 3},
		{},
		{6, nil, &root},
	}
	fmt.Println(nodes) // [{3 <nil> <nil>} {0 <nil> <nil>} {6 <nil> 0xc000086020}]
}
```

### 结构的创建

 - 使用自定义工厂函数
 - 注意返回了局部变量的地址，go语言是可以用的

```go
func CreateNode(value int) *treeNode {
	return &treeNode{value: value}
}

root.left.right = CreateNode(2)
```

### 结构创建在堆上还是栈上？

 - 不需要知道，有垃圾回收，别人使用就分配在堆上，没有就分配在栈上

 ### 为结构定义方法

  - 显示定义和命名方法接受者

```go
func (node treeNode) print() { // 传值  要值 地址可以自动取值传进去
	fmt.Print(node.value, " ")
}
```

### 使用指针作为方法接受者

 - 只有使用指针才可以改变结构内容
 - nil指着也可以调用方法！
```go
func (node *treeNode) setV(value int) {
	node.value = value
}
```

```go
package main

import "fmt"

type treeNode struct {
	value       int
	left, right *treeNode
}


func CreateNode(value int) *treeNode {
	return &treeNode{value: value}
}
func (node treeNode) print() { // 传值  要值 地址可以自动取值传进去
	fmt.Print(node.value, " ")
}
func (node treeNode) setValue(value int) { // 传值
	node.value = value
}

func (node *treeNode) setV(value int) {
	if node == nil {
		fmt.Print("Setting value to nil " + "node. Ignored. ")
		return
	}
	node.value = value
}
func main() {
	var root treeNode
	root = treeNode{value: 3}
	root.left = &treeNode{}
	root.right = &treeNode{5, nil, nil}
	root.right.left = new(treeNode) //c++ root.Right->left
	root.left.right = CreateNode(2)
	nodes := [] treeNode {
		{value: 3},
		{},
		{6, nil, &root},
	}
	fmt.Println(nodes) // [{3 <nil> <nil>} {0 <nil> <nil>} {6 <nil> 0xc000086020}]
	root.right.left.setValue(4) //传值
	root.right.left.print()// 0
	root.right.left.setV(4)
	root.right.left.print()// 4
	root.print()//3


	var pRoot *treeNode
	pRoot.setV(200) // Setting value to nil node. Ignored.
	pRoot = &root
	pRoot.print()// 3
	pRoot.setV(200)
	pRoot.print()// 200
}
```

### 遍历

```go
func (node *treeNode) traverse() {
	if node == nil {
		return
	}
	node.left.traverse()
	node.print()
	node.right.traverse()
}
```

### 使用指针作为方法接受者

 - 只有使用指针才可以改变结构内容
 - nil指针也可以调用方法！

 ### 值接受者 vs 指针接受者

 - 要改变内容必须使用指针接受收者
 - 结构过大也考虑使用指针接收者 值拷贝性能问题
 - 一致性：如有指针接受者，最好都是指针接收者
 - 值接收者是go语言特有
 - 值/指针接收者均可接收值/指针

### 封装

 - 名字一般使用CamelCase
 - 首字母大写：public -> 针对包来说
 - 首字母小写：private -> 针对包来说

### 包

 - 每个目录一个包
 - main包包含可执行入口
 - 为结构定义的方法必须放在同一个包内
 - 可以是不同的文件

```go
// test/tree/node.go
package tree

import "fmt"

type Node struct {
	Value       int
	Left, Right *Node
}

func (node Node) Print() {
	fmt.Print(node.Value, " ")
}

func (node *Node) SetValue(value int) {
	if node == nil {
		fmt.Println("Setting Value to nil " +
			"node. Ignored.")
		return
	}
	node.Value = value
}

func CreateNode(value int) *Node {
	return &Node{Value: value}
}

// test/tree/traversal.go
package tree

func (node *Node) Traverse() {
	if node == nil {
		return
	}
	node.Left.Traverse()
	node.Print()
	node.Right.Traverse()
}

// test/tree/entry/entry.go
package main

import (
	"binperson/test/tree"
)

type myTreeNode struct {
	node *tree.Node
}

func (myNode *myTreeNode) postOrder() {
	if myNode == nil || myNode.node == nil {
		return
	}

	left := myTreeNode{myNode.node.Left}
	right := myTreeNode{myNode.node.Right}

	left.postOrder()
	right.postOrder()
	myNode.node.Print()
}

func main() {
	var root tree.Node

	root = tree.Node{Value: 3}
	root.Left = &tree.Node{}
	root.Right = &tree.Node{5, nil, nil}
	root.Right.Left = new(tree.Node)
	root.Left.Right = tree.CreateNode(2)
	root.Right.Left.SetValue(4)
	root.Traverse() // 0 2 3 4 5
}
```

### 如何扩充系统类型或者别人的类型

 - 定义别名
 - 使用组合

```go

// 组合的方式
package main

import (
	"binperson/test/tree"
)

type myTreeNode struct {
	node *tree.Node
}

func (myNode *myTreeNode) postOrder() {
	if myNode == nil || myNode.node == nil {
		return
	}

	left := myTreeNode{myNode.node.Left}
	right := myTreeNode{myNode.node.Right}

	left.postOrder()
	right.postOrder()
	myNode.node.Print()
}

func main() {
	var root tree.Node

	root = tree.Node{Value: 3}
	root.Left = &tree.Node{}
	root.Right = &tree.Node{5, nil, nil}
	root.Right.Left = new(tree.Node)
	root.Left.Right = tree.CreateNode(2)
	root.Right.Left.SetValue(4)
	root.Traverse() // 0 2 3 4 5

	myRoot := myTreeNode{&root}
	myRoot.postOrder() // 2 0 4 5 3 
}
```

```go
// 定义别名
package queue

// A FIFO queue.
type Queue []int

// Pushes the element into the queue.
// 		e.g. q.Push(123)
func (q *Queue) Push(v int) {
	*q = append(*q, v)
}

// Pops element from head.
func (q *Queue) Pop() int {
	head := (*q)[0]
	*q = (*q)[1:]
	return head
}

// Returns if the queue is empty or not.
func (q *Queue) IsEmpty() bool {
	return len(*q) == 0
}


//
package main

import (
	"binperson/test/queue"
	"fmt"
)

func main() {
	q := queue.Queue{1}

	q.Push(2)
	q.Push(3)
	fmt.Println(q.Pop())
	fmt.Println(q.Pop())
	fmt.Println(q.IsEmpty())
	fmt.Println(q.Pop())
	fmt.Println(q.IsEmpty())

}
```

### GOPATH环境变量

 - 默认在～/go(unix,linx), %USERPROFILE%\go(windows)
 - 官方推荐：所有项目和第三方库都放在同一GOPATH下
 - 也可以将每个项目放在不同的GOPATH

 ```
 echo $GOPATH
 cat ~/.bash_profile
 ```

### go get 获取第三方库

 - go get 命令演示
 - 使用gopm来获取无法下载的包
 - go build 来编译
 - go install来产生pkg文件和可执行文件
 - go run直接编译运行

```
go get -v github.com/gpmgo/gopm

gopm get -g -v ~~~
gopm get -g -u -v ~~~
go build
go install
```

### GOPATH下目录结构
 
 - src
  - git repository 1
  - git repository 2
 - pkg
  - git repository 1
  - git repository 2
 - bin 
  - 执行文件1，2，3...