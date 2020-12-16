### 变量
---------------------
声明变量的一般形式是使用 var 关键字
 * 指定变量类型，若没有初始化，会指定默认值
 * ' := '是var声明变量的另一种写法，注意' := ''左侧必须声明新的变量
 
 
 以下是对变量定义的简单尝试
		
```go
package main

import "fmt"

// 全局变量
var (
	param1 bool
	param2 = 2
)

func main() {
	// var + 变量赋值
	var a string = "Go语言打卡学习"
	fmt.Println(a)
	
	// b默认值为 0
	var b int
	fmt.Println(b)
	
	// := 变量定义
	c, d := 2, 3
	fmt.Println(c + d)

	fmt.Println(param1, param2)
}

```


### 常量
----------------------
常量是一个简单值的标识符，在程序运行时，不会被修改的量。数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型；

	const identifier [type] = value
	const b = "abc"

```go
package main

import "fmt"

//全局常量
const (
	param1 int  = 5
	param2 bool = false 
)

func main() {
	const a = 10
	fmt.Println(a, param1, param2)

}
```

* iota 特殊常量

在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次
```go
package main

import "fmt"

func main() {
	const (
		a = 1 + iota
		b = 3 + iota
		c
	)
	fmt.Println(a, b, c)

}
//输出为1,4,5 为(1+0),(3+1),(3+2)

```

### 枚举
-------------
枚举，将变量的值一一列举出来，变量只限于列举出来的值的范围内取值。Go语言中没有枚举这种数据类型的，但是可以使用const配合iota模式来实现

* 普通枚举

		const (
	            a = 0
				b = 1
	            c = 2
	            d = 3
                     )

* 自增枚举

        const (
	           a = iota //0
	           c        //1
	           d        //2
	           e = 100  //100
	           f        //100
	           g = iota //5
        )
        const (
	          e, f = iota, iota //e=0, f=0
	          g    = iota       //g=1
        )



