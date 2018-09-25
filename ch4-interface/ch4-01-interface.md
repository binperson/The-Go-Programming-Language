### 接口


### duck typing

 - 大黄鸭是鸭子吗？
  - 传统类型系统：脊索动物门。。。
  - duck typing：是鸭子
  - 像鸭子走路，想鸭子叫（长得像鸭子），那么就是鸭子
  - 描述事物的外部行为而非内部结构
  - 严格说go属于结构化类型系统，类似duck typing

### python中的duck typing

```python
def download(retriever):
    return retriever.get("www.binperson.com")
```

 - 运行时才知道传人的retriever有没有get
 - 需要注释来说明接口

### c++中的duck typing

```c++
template <class R>
string download(const R& retriever) {
    return retriever.get("www.binperson.com")
}
```

 - 编译时才知道传人的retriever有没有get
 - 需要注释来说明接口

### java中的类似代码

```java
<R extends Retriever>
String download(R r) {
    return r.get("www.binperson.com")
}
```

 - 传人的参数必须实现Retriever接口
 - 不是duck typing

### go语言的duck typing

 - 同时需要Readable，Appendable怎么办？（apache polygene）
 - 同时具有python，c++的duck typing的灵活性
 - 又具有java的类型检查

### 接口的定义

 - 接口由使用者定义

```go
//接口的定义
type Retriever interface {
	Get(url string) string // interface 不用加func关键字 因为里面全是函数
} 
func dowload(r Retriever)  string{
	return r.Get("http://www.baidu.com")
}
```


```go
//
package mock

type Retriever struct {
	Contents string
}

func (r Retriever) Get(url string) string {
	return r.Contents
}



//
package main

import (
	"fmt"
	"binperson/test/retriever/mock"
)

type Retriever interface {
	Get(url string) string // interface 不用加func关键字 因为里面全是函数
} 
func dowload(r Retriever)  string{
	return r.Get("www.binperson.com")
}
func main()  {
	var r Retriever
	r = mock.Retriever{"this is binperson.com"}
	fmt.Print(dowload(r))

}
```

### 接口的实现

 - 接口的实现是隐式的
 - 只要实现接口里的方法

```go
//
package mock

type Retriever struct {
	Contents string
}

func (r Retriever) Get(url string) string {
	return r.Contents
}

//

package real

import (
	"time"
	"net/http"
	"net/http/httputil"
)

type Retriever struct {
	UserAgent string
	TimeOut   time.Duration
}

func (r *Retriever) Get(url string) string {
	resp, err := http.Get(url)
	if err != nil {
		panic(err)
	}
	result, err := httputil.DumpResponse(
		resp, true)

	resp.Body.Close()

	if err != nil {
		panic(err)
	}

	return string(result)
}


//
package main

import (
	"fmt"
	"binperson/test/retriever/mock"
	"binperson/test/retriever/real"
	"time"
)

type Retriever interface {
	Get(url string) string // interface 不用加func关键字 因为里面全是函数
} 
func dowload(r Retriever)  string{
	return r.Get("http://www.baidu.com")
}
func main()  {
	var r Retriever
	r = mock.Retriever{"this is binperson.com"}
	fmt.Printf("%T %v\n", r, r) // mock.Retriever {this is binperson.com}
	fmt.Print(dowload(r)) // this is binperson.com
	r = &real.Retriever{
		UserAgent: "Mozilla/5.0",
		TimeOut: time.Minute,
	}
	fmt.Printf("%T %v\n", r, r) // real.Retriever {Mozilla/5.0 1m0s}
    fmt.Print(dowload(r))
    
}

```

### 接口变量里面有什么

 - 接口变量
  - 实现者的类型
  - 实现者的值
- 接口变量
  - 实现者的类型
  - 实现者的指针 -> 实现者

### 接口变量里面有什么

 - 接口变量自带指针
 - 接口变量同样采用值传递，几乎不需要使用接口的指针
 - 指针接收者实现只能以指针方式使用，值接收者都可

### 查看接口变量

 - 表示任何类型: interface{}
 - Tpye Assertion
 - Type Switch

```go
package main

import (
	"fmt"
	"binperson/test/retriever/mock"
	"binperson/test/retriever/real"
	"time"
)

type Retriever interface {
	Get(url string) string // interface 不用加func关键字 因为里面全是函数
} 

func dowload(r Retriever)  string{
	return r.Get("http://www.baidu.com")
}
func main()  {
	var r Retriever
	r = mock.Retriever{"this is binperson.com"}
	inspect(r)
	/*
	mock.Retriever {this is binperson.com}
	Contents: this is binperson.com
	*/
	r = &real.Retriever{
		UserAgent: "Mozilla/5.0",
		TimeOut: time.Minute,
	}
	/*
	*real.Retriever &{Mozilla/5.0 1m0s}
	UserAgent: Mozilla/5.0
	*/
	inspect(r)

	// Tpye Assertion
	realRetriever := r.(*real.Retriever)
	fmt.Println(realRetriever.TimeOut) // 1m0s
	if mockRetriever, ok := r.(mock.Retriever); ok {
		fmt.Println(mockRetriever.Contents)
	} else {
		fmt.Println("not a mockRetriever")
	}
}

func inspect(r Retriever) {
	fmt.Printf("%T %v\n", r, r)
	switch v := r.(type) {
	case mock.Retriever:
		fmt.Println("Contents:", v.Contents)
	case *real.Retriever:
		fmt.Println("UserAgent:", v.UserAgent)
	}
}
```

### 接口组合 and 系统接口


### 特殊接口

 - Stringer
 - Reader/Writer