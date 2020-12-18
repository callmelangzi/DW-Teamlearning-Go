## 数组切片
### 数组
```go
//数组定义

//方式一
var arr1 = [5]int{} //[0 0 0 0 0 ]
//方式二
var arr2 = [5]int{1,2,3,4,5} // [1 2 3 4 5]
//方式三
var arr3 = [5]int{3:10} // [0 0 0 10 0]
```
* 方法一在声明时没有为其指定初值，所以数组内的值被初始化为类型的零值
* 方法二使用显示的方式为数组定义初值
* 方法三通过下标的方式为下标为3的位置赋上了初值10

数组的定义是包含其长度的，也就是说[3]int与[4]int是两种不同的数据类型

#### 给数组赋值
```go
package main

import (
	"fmt"
)

func main() {
	var arr1 = [5]int{}
	for i := 0; i < len(arr1); i++ {
		arr1[i] = i * 10
	}
	fmt.Println(arr1)
    
    // 遍历数组
    for index, value := range arr1 {
		fmt.Println(index, value)
	}
	
}

```

#### 多维数组
```go

var arr = [5][5]int{
		{1, 2, 3, 4, 5},
		{6, 7, 8, 9, 10},
	}
// [[1 2 3 4 5] [6 7 8 9 10] [0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0]]
```

#### 数组作为函数参数
go语言在传递数组时会对其进行拷贝，所以如果传递的是大数组的话会非常占内存，所以一般情况下很少直接传递一个数组，避免这种情况我们可以使用以下两种方式
* 传递数组的指针
* 传递切片

###  指针数组与数组指针
####  指针数组
对于指针数组来说，就是一个数组里面装的都是指针，在go语言中数组默认是值传递的，所以如果我们在函数中修改传递过来的数组对原来的数组是没有影响的
```go
package main

import (
	"fmt"
)

func main() {
	var a [5]int
	fmt.Println(1, a) // 1,[0 0 0 0 0]
	test(a)
	fmt.Println(3, a) // 3,[0 0 0 0 0] 
}

func test(a [5]int) {
	a[1] = 1
	fmt.Println(2, a) // 2,[0 1 0 0 0 ]
}
}
```

#### 函数传递指针数组
```go
package main

import "fmt"

func main() {
	var a [5]*int
	fmt.Println(1, a)
	for i := 0; i < 5; i++ {
		temp := i
		a[i] = &temp
	}
	for i := 0; i < 5; i++ {
		fmt.Print(" ", *a[i]) // [0 1 2 3 4]
	}
	fmt.Println()
	test1(a)
	for i := 0; i < 5; i++ {
		fmt.Print(" ", *a[i]) // [0 2 2 3 4]
	}
}

func test1(a [5]*int) {
	*a[1] = 2
	for i := 0; i < 5; i++ {
		fmt.Print(" ", *a[i]) // [0 2 2 3 4]
	}
	fmt.Println()
}

```
初始化值全是nil，验证了指针数组内部全都是一个一个指针，之后我们将其初始化，内部的每个指针指向一块内存空间。

注：在初始化的时候如果直接另a[i] = &i那么指针数组内部存储的全是同一个地址，所以输出结果也一定是相同的

然后我们将这个指针数组传递给test1函数，对于数组的参数传递仍然是复制的形式也就是值传递，但是因为数组中每个元素是一个指针，所以test1函数复制的新数组中的值仍然是这些指针指向的具体地址值，这时改变a[1]这块存储空间地址指向的值，那么原实参指向的值也会变为2

#### 数组指针
数组指针是指向数组的指针，在go语言中我们可以如下操作
```go
func main() {
	var a [5]int
	var aPtr *[5]int
	aPtr = &a
	//这样简短定义也可以aPtr := &a
	fmt.Println(aPtr)
	test(aPtr)
	fmt.Println(aPtr)
}


func test(aPtr *[5]int) {
	aPtr[1] = 5
	fmt.Println(aPtr)
}

```

我们先定义了一个数组a，然后定一个指向数组a的指针aPtr，然后将这个指针传入一个函数，在函数中我们改变了具体的值，程序的输出结果

	&[0 0 0 0 0]
    &[0 5 0 0 0]
    &[0 5 0 0 0]
虽然main和test函数中的aPtr是不同的指针，但是他们都指向同一块数组的内存空间，所以无论在main函数还是在test函数中对数组的操作都会直接改变数组


### 	切片
因为数组是固定长度的，所以在一些场合下就显得不够灵活，所以go语言提供了一种更为便捷的数据类型叫做切片。切片操作与数组类似，但是它的长度是不固定的，可以追加元素，如果以达到当前切片容量的上限会再自动扩容。

#### 切片定义
```go
//方法一
var s1 = []int{}
//方法二
var s2 = []int{1, 2, 3}
//方法三
var s3 = make([]int, 5)
//方法四
var s4 = make([]int, 5, 10)

```

方法一声明了一个空切片，方法二声明了一个长度为3的切片，方法三声明了一个长度为5的空切片，方法四声明了一个长度为5容量为10的切片。可以看到切片的定义与数组类似，但是定义切片不需要为其指定长度。

我们可以通过len()和cap()这两个函数来获取切片的长度和容量

#### 切片操作(左闭右开)
```go
func main() {
	arr := [5]int{1, 2, 3, 4, 5}
	s := []int{6, 7, 8, 9, 10}

	s1 := arr[2:4]
	s2 := arr[:3]
	s3 := arr[2:]
	s4 := s[1:3]

	fmt.Println("s1:", s1)// s1: [3 4]
	fmt.Println("s2:", s2)// s2: [1 2 3]
	fmt.Println("s3:", s3)// s3: [3 4 5]
	fmt.Println("s4:", s4)// s4: [7 8]
}
```

#### 切片的扩充与拼接
```go
func main() {
	a := []int{1, 2, 3}
	b := a[1:3]

	b = append(b, 4)
	b = append(b, 5)
	b = append(b, 6)
	b = append(b, 7)
	fmt.Println(a) //[1 2 3]
	fmt.Println(b) //[2 3 4 5 6 7]
}
```

#### 切片拼接
```go

func main() {
	a := []int{1, 2, 3}
	b := a[1:3]

	fmt.Println(b)

	a = append(a, b...)
	fmt.Println(a)
}
```

#### 切片赋值
```go
func main() {
	a := []int{1, 2, 3}
	b := make([]int, 3)
	copy(b, a)
	fmt.Println(a)
	fmt.Println(b)
}
```
