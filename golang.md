# 云柚科技Golang语言编码规范

状态: 草稿

更新时间: 2016/02/02

## 工具使用

Golang编程过程中代码建议使用goimports[^goimports]工具进行代码统一格式化与import管理。该工具可以通过GoSublime[^GoSublime]或者go-plus[^go-plus]等工具与你的编辑器进行直接整合。

代码单元测试，统一使用goconvey[^goconvey]工具进行测试管理。

## 命名规范

### 包命名

Golang包使用小写英文字母进行命名，应不包含大写、数字和特殊符号。

```go
import "bytes"
```

### 接口命名

接口命名一般以```er```结尾，如```Reader```、```Writer```等。实现方法不出现```er```结尾，如```Read```、```Write```等。

```go
type Reader interface {
	Read(p []byte) (n int, err error)
}
```

### 变量/函数命名

变量命名统一采用驼峰命名法，全局变量根据实际情况决定是否大写。其余情况下首写字母均为小写。

```go
var count, Sum int
```

### 接收者命名

对于结构的接收者方法，应避免结构名的出现。同时，接收者名称统一采用第一个字母的小写表示。接收者除非知道作用，否则同一使用```*```类型。

```go
type newStruct struct{ ...

func (n *newStruct) Get(){ ...
```

## 注释

### Package 的注释

对于提供给多个项目进行使用的公共包，需对包文件进行整体注释，方便在godoc中进行查看。

```go
//Package regexp implements a simple library 
//for regular expressions.
package regexp 
```

### 导出函数／类型的注释

对于对外提供的函数、类型等信息需要提供注释，用于描述对应功能。描述起始使用名称表示开头。

```go
// A Request represents a request to run a command.
type Request struct { ...

// Encode writes the JSON encoding of req to w.
func Encode(w io.Writer, req *Request) { ...
```

### 缩写避免使用大小写形式

对于缩写，如```ID```、```URL```等，应采用大写形式。

## 错误处理

### 错误及时处理

避免出现使用```_```忽略错误的情况，对于错误应及时进行返回。

```go
if err := file.Chmod(0664); err != nil {
    return err
}

...

if err != nil {
   	// error handling
}
// normal code
```

### 返回值中带有error

非顶层包需要在可能出现异常情况的地方返回error给上层协助故障诊断。顶层包采用日志记录方式记录出现的异常数据。

### 避免使用panic

除非你遇到了完全未知的错误，这个错误仅能由程序自己产生，一般不允许手动进行。

## 变量必要时分配

尽量对```slice```、```map[string]xxxx```类型的数据在使用之前才进行必要的分配。

```go
var t []string
```

## 传参

普通变量参数应避免指针参数传递，除非你是在传递大量数据struct或者你需要修改参数中的某些值到外部。

## 函数返回值

对于函数返回值，不含同一类型的，可以选择不命名方式表示。对于存在相同类型的返回值，为了区分，建议进行命名。

```go
func (n *Node) Parent1() *Node
func (n *Node) Parent2() (*Node, error)

// Location returns f's latitude and longitude.
// Negative values mean south and west, respectively.
func (f *Foo) Location() (lat, long float64, err error)
```

## 参考文档

1. [Effective Go](https://golang.org/doc/effective_go.html)

2. [Google Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

[^goimports]: [https://golang.org/x/tools/cmd/goimports](https://golang.org/x/tools/cmd/goimports)

[^goconvey]: [https://github.com/smartystreets/goconvey](https://github.com/smartystreets/goconvey)

[^GoSublime]: [https://github.com/DisposaBoy/GoSublime](https://github.com/DisposaBoy/GoSublime)

[^go-plus]: [https://github.com/joefitzgerald/go-plus](https://github.com/joefitzgerald/go-plus)