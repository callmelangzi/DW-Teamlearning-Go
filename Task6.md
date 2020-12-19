### 函数
----------------
#### 函数定义

```go
func functionName([parameter list]) [returnTypes]{
   //body
}
```
* 函数由func关键字进行声明
* functionName：代表函数名
* parameter list：代表参数列表，函数的参数是可选的，可以包含参数也可以不包含参数
* returnTypes：返回值类型，返回值是可选的，可以有返回值，也可以没有返回值
* body：用于写函数的具体逻辑

##### a+b函数实现
```go
package main

import "fmt"

// a+b
func GetSum(num1, num2 int) int {
	res := num1 + num2
	return res
}

func main() {
	fmt.Println(GetSum(2, 3))
}
```

#### 值传递与引用传递
在go语言中存在值类型与引用类型，所以在函数参数进行传递时也要注意这个问题
* 值传递是指在函数调用过程中将实参拷贝一份到函数中，这样在函数中如果对参数进行修改，将不会影响到实参
* 引用传递是指在函数调用过程中将实参的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到实参

如果想要函数可以直接修改参数的值，那么我们可以用指针传递，将变量的地址作为参数传递到函数中。

##### 实例说明
```go
package main

import "fmt"

func paramFunc(a int, b *int, c []int) {
	a = 100
	*b = 100
	c[1] = 100

	fmt.Println("paramFunc:")
	fmt.Println(a)
	fmt.Println(*b)
	fmt.Println(c)
}

func main() {
	a := 1
	b := 1
	c := []int{1, 2, 3}
	paramFunc(a, &b, c) 
	//paramFunc:
	//100
	//100
	//[1 100 3]

	fmt.Println("main:") // main:
	fmt.Println(a) // 1 
	fmt.Println(b) // 100 
	fmt.Println(c) // [1 100 3]
}


```

####  变长参数
在go语言中也支持变长参数，但需要注意的是变长参数必须放在函数参数的最后一个，否则会报错
##### 实例说明
```go
package main

import "fmt"

func main() {
	slice := []int{7, 9, 3, 5, 1}
	x := min(slice...)
	fmt.Println(x) // 1
}

func min(s ...int) int { //func min(s []int) int { 
	if len(s) == 0 {
		return 0
	}
	min := s[0]
	for _, v := range s {
		if v < min {
			min = v
		}
	}
	return min

}


```

#### 多返回值
go语言中函数还支持一个特性那就是：多返回值。通过返回结果与一个错误值，这样可以使函数的调用者很方便的知道函数是否执行成功，这样的模式也被称为command,ok模式

##### 实例
```go
package main

import (
	"errors"
	"fmt"
)

func div(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("The divisor cannot be zero.")
	}
	return a / b, nil
}

func main() {
	result, err := div(1, 0)
	if err != nil {
		fmt.Printf("error: %v", err)
		return
	}
	fmt.Println("result: ", result)
}


```

#### 命名返回值
在go语言中还可以给返回值命名，当需要返回的时候，我们只需要一条简单的不带参数的return语句

###### 实例
```go
package main

import (
	"errors"
	"fmt"
)

// func div(a, b float64) (float64, error) {
// 	if b == 0 {
// 		return 0, errors.New("The divisor cannot be zero.")
// 	}
// 	return a / b, nil
// }

func div(a, b float64) (result float64, err error) { //区别1
	if b == 0 {
		return 0, errors.New("被除数不能等于0")
	}
	result = a / b
	return //区别2
}

func main() {
	result, err := div(1, 2)
	if err != nil {
		fmt.Printf("error: %v", err)
		return
	}
	fmt.Println("result: ", result)
}


```

#### 匿名函数
匿名函数，是一个没有名字的函数，除了没有名字外其他地方与正常函数相同。匿名函数可以直接调用，保存到变量，作为参数或者返回值
##### 实例
```go
package main

import (
	"fmt"
)

func main() {
	f := func() string {
		return "hello world"
	}
	fmt.Println(f())
}

```

#### 闭包
闭包可以解释为一个函数与这个函数外部变量的一个封装。粗略的可以理解为一个类，类里面有变量和方法，其中闭包所包含的外部变量对应着类中的静态变量

##### 实例
```go
package main

import (
	"fmt"
	"strconv"
)

func add() func(int) int {
	n := 10
	str := "string"
	return func(x int) int {
		n = n + x
		str += strconv.Itoa(x)
		fmt.Print(str, " ")
		return n
	}
}

func main() {
	f := add()
	fmt.Println(f(1)) // string1 11
	fmt.Println(f(2)) // string12 13 
	fmt.Println(f(3)) // string123 16

	f = add()
	fmt.Println(f(1)) // string1 11
	fmt.Println(f(2)) // string12 13
	fmt.Println(f(3)) // string123 16
}


```

闭包就是一个函数和一个函数外的变量的封装，而且这个变量就对应着类中的静态变量
* 最开始我们先声明一个函数add，在函数体内返回一个匿名函数
* 其中的n,str与下面的匿名函数构成了整个的闭包，n与str就像类中的静态变量只会初始化一次，所以说尽管后面多次调用这个整体函数，里面都不会再重新初始化了
* 而且对于外部变量的操作是累加的，这与类中的静态变量也是一致的


##### 使用闭包实现斐波那契
```go
package main

import (
	"fmt"
)

func fei() func() int {
	a := 0
	b := 1
	return func() int {
		a, b = b, a+b
		return b
	}
}

func main() {
	f := fei()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}

}

```
