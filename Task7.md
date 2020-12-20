### 结构体、方法、接口 
-----------

#### 结构体
Go语言中没有“类”的概念，也不支持像继承这种面向对象的概念。但是Go 语言的结构体与“类”都是复合结构体，而且Go 语言中结构体的组合方式比面向对象具有更高的扩展性和灵活性。

##### 结构体定义
```go

type identifier struct {
  field1 type1
  field2 type2
  ...
}
// 结构体实例
type student  struce{
   name string
   age int 
   sex string
}
//结构体中字段的类型可以是任何类型，包括函数类型，接口类型，甚至结构体类型本身。例如我们声明一个链表中的节点的结构体类型。

type ListNode struce {
    Val int
    Next *ListNode
}


//在声明结构体时我们也可以不给字段指定名字

type Person struce {
    Id string
    int // 可以看到其中有一个int字段没有名字，这种我们称其为匿名字段
}
```

##### 结构体实列
```go

package main

import "fmt"

type student struct {
	name string
	age  int
}

type person struct {
	id string
	int // 匿名函数
}

func main() {
    // a 正常结构体定义
	a := student{name: "ABC", age: 20}
	fmt.Println(a)
	// b 匿名函数结构体
	b := person{"hello go", 7}
	fmt.Println(b)

}

```

#####  操作结构体
```go
// 结构体声明
s1 := new(Student) //第一种方式
s2 := Student{"james", 35} //第二种方式
s3 := &Student { //第三种方式
	Name: "LeBron",
	Age:  36,
}
```
* 使用new函数会创建一个指向结构体类型的指针，创建过程中会自动为结构体分配内存，结构体中每个变量被赋予对应的零值
* 也可以使用第二种方式生命结构类型，需要注意的是此时给结构体赋值的顺序需要与结构体字段声明的顺序一致
* 第三种方式更为常用，我们创建结构体的同时显示的为结构体中每个字段进行赋值

##### 结构体赋值
```go
s1.Name = "james"
s1.Age = 35


//上面提到的匿名字段，可以使用如下方法对其进行操作
p := new(Person)
p.ID = "123"
p.int = 10

```
* 需要注意的是，结构体也仍然遵循可见性规则，要是定义结构体的字段时首字母为小写在其他包是不能直接访问该字段的。


##### 标签
在go语言中结构体除了字段的名称和类型外还有一个可选的标签tag，标记的tag只有reflect包可以访问到，一般用于orm或者json的数据传递，下面这段代码演示了如何为结构体打标签
```go
type Student struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```
我们可以使用go自带的json包将声明的结构体变量转变为json字符串。
```go
func ToJson(s *Student) (string, error) {
	bytes, err := json.Marshal(s)
	if err != nil {
		return "", nil
	}
	return string(bytes), nil
}
```

##### 内嵌结构体
之前我们介绍到了匿名字段，结构体作为一种数据类型也可以将其生命为匿名字段，此时我们称其为内嵌结构体，下面这段代码中我们将结构体A嵌入到结构体B中。
```go

type A struct {
	X, Y int
}

type B struct {
	A
	Name string
}
```
通过内嵌结构体的方式我们可以在结构体B的变量下很方便的操作A中定义的字段。
```go
b := new(B)
b.X = 10
b.Y = 20
b.Name = "james"
```


##### 内嵌结构体实操
```go
package main

import "fmt"

type A struct {
	X, Y int
}

type B struct {
	A
	name string
}

func main() {
	a := B{}
	a.X = 10
	a.Y = 20
	a.name = "Hello"

	fmt.Println(a)

}

```
------------
#### 方法
##### 方法定义
方法与函数类似，只不过在方法定义时会在func和方法名之间增加一个参数
```go
func (r Receiver)func_name(){
  // body
}
//其中r被称为方法的接收者

//实例
type Person struct {
	name string
}

func (p Person) GetName() string {
	return p.name
}
//其中GetName方法的接收者为p是Person结构体类型，也就是说我们为结构体Person绑定了一个GetName方法，我们可以使用如下的方式进行调用。
func main() {
	p := Person{
		name:"james",
	}
	fmt.Println(p.GetName())
}
```

##### 方法接收者
对于一个方法来说接收者分为两种类型：值接收者和指针接收者。上面的GetName的接收者就是值接收者。我们再为Person结构体定义一个指针接收者
```go
func (p *Person)SetName(name string){
	p.name = name
}
```
使用值接收者定义的方法，在调用的时使用的其实是值接收者的一个拷贝，所以对该值的任何操作，都不会影响原来的类型变量。

但是如果使用指针接收者的话，在方法体内的修改就会影响原来的变量，因为指针传递的也是地址，但是是指针本身的地址，此时拷贝得到的指针还是指向原值的，所以对指针接收者操作的同时也会影响原来类型变量的值。

而且在go语言中还有一点比较特殊，我们使用值接收者定义的方法使用指针来调用也是可以的，反过来也是如此，如下所示：

```go
func main() {
	p := &Person{
		name: "james",
	}
	fmt.Println(p.GetName())

  p1 := Person{
		name: "james",
	}
	p1.SetName("kobe")
	fmt.Println(p1.GetName())
}
```

-----------
#### 接口
##### 接口定义
接口相当于一种规范，它需要做的是谁想要实现我这个接口要做哪些内容，而不是怎么做。在go语言中接口的定义如下所示
```go

type Namer interface {
    Method1(param_list) return_type
    Method2(param_list) return_type
    ...
}
```

##### 实现接口
在go语言中不需要显示的去实现接口，只要一个类型实现了该接口中定义的所有方法就是默认实现了该接口，而且允许多个类型都实现该接口，也允许一个类型实现多个接口。
```go
type Animal interface {
	Eat()
}//接口

type Bird struct {
	Name string
}//结构体

func (b Bird) Eat() {
	fmt.Println(b.Name + "吃虫")
}//方法

type Dog struct {
	Name string
}

func (d Dog) Eat() {
	fmt.Println(d.Name + "吃肉")
}

func EatWhat(a Animal) {
	a.Eat()
}

func main() {
	b := Bird{"Bird"}
	d := Dog{"Dog"}
	EatWhat(b)
	EatWhat(d)
}
```

在EatWaht函数中是传递一个Animal接口类型，上面的Bird和Dog结构体都实现了Animal接口，所以都可以传递到函数中去来实现多态特性


##### 类型断言
有些时候方法传递进来的参数可能是一个接口类型，但是我们要继续判断是哪个具体的类型才能进行下一步操作，这时就用到了类型断言
```go

func IsDog(a Animal) bool {
	if v, ok := a.(Dog); ok {
		fmt.Println(v)
		return true
	}
	return false
}
```
上面的方法对传递进来的参数进行判断，判断其是否为Dog类型，如果是Dog类型的话就会将其进行转换为v，ok用来表示是否断言成功。但是如果我们对于一个类型有好多种子类型要进行判断，这样写的话显然是有些复杂

```go

func WhatType(a Animal) {
	switch a.(type) {
	case Dog:
		fmt.Println("Dog")
	case Bird:
		fmt.Println("Bird")
	default:
		fmt.Println("error")
	}
}
```

##### 空接口
空接口是一个比较特殊的类型，因为其内部没有定义任何方法所以空接口可以表示任何一个类型
```go

var any interface{}

any = 1
fmt.Println(any)

any = "hello"
fmt.Println(any)

any = false
fmt.Println(any)
```
