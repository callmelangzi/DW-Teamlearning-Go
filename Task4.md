## 字典and字符串
--------------
### 字典
map是一种较为特殊的数据结构，在任何一种编程语言中都可以看见他的身影，它是一种键值对结构，通过给定的key可以快速获得对应的value

#### 字典定义
在定义字典时不需要为其指定容量，因为map是可以动态增长的，但是在可以预知map容量的情况下为了提高程序的效率也最好提前标明程序的容量。需要注意的是，不能使用不能比较的元素作为字典的key，例如数组，切片等


```go
package main

import "fmt"

func main() {
	var m1 map[string]int
	m1 = make(map[string]int)
	m1["a"] = 1
	m1["b"] = 2
	fmt.Println(m1)
    // 没有指定值的类型时，尝试使用了字符串类型和整数类型，可以正常输出
	m2 := make(map[string]interface{}, 10)
	m2["c"] = "a"
	m2["d"] = 2
	fmt.Println(m2)

    //输出字典长度
	fmt.Println(len(m2))
	// 按键查找值
	fmt.Println(m2["a"])

	//遍历字典
	for key, value := range m2 {
		fmt.Println("Key is: ", key, "Value is: ", value)
	}

    //删除字典键值对
    delete(m2,"c")
    
}

```

##### leetcode两数相加使用字典AC
```go
func twoSum(nums []int, target int) []int {
    hash_map := make(map[int] int)
    for i, j:= range nums{
        if p,ok := hash_map[target - j];ok{
            return [] int{p,i}
        }
        hash_map[j] = i  
    }
    return nil
}

```
* 练习了字典操作以及循环相关知识


### 字符串
字符串是一种值类型，在创建字符串之后其值是不可变的，如果我们想要修改一个字符串的内容，我们可以将其转换为字节切片，再将其转换为字符串，但是也同样需要重新分配内存。

```go

package main

import "fmt"

func main() {
	s := "hello"
	b := []byte(s)
	// 单引号和双引号老是搞错..python写多了
	b[0] = 'H'
	b[1] = 'E'
	s = string(b)
	fmt.Println(s)


}

```
####  含中文的切片操作

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	s := "Go你好"
	fmt.Println(len(s))                    //8
	fmt.Println(utf8.RuneCountInString(s)) //4

	b := []byte(s)
	for i := range b {
		fmt.Printf("%c", b[i])
	}
	// for i := 0; i < len(b); i++ {
	// 	fmt.Printf("%c", b[i])
	// }
	fmt.Printf("\n")

	c := []rune(s)
	for i := range c {
		fmt.Printf("%c", c[i])
	} // Go你好

	fmt.Printf("\n")

	// for i := 0; i < len(c); i++ {
	// 	fmt.Printf("%c", c[i])
	// }
	// fmt.Printf("\n")

}

```

#### 字符串拼接

字符串拼接也是很常用的一种操作，在go语言中有多种方式可以实现字符串的拼接

```go
package main

import "fmt"

func main() {
	a, b := "hello, ", "Go"
	//使用 sprintf 拼接
	a = fmt.Sprintf("%v%v", a, b)
	fmt.Println(a)

    //使用 + 进行拼接 只能拼接int类型？
    a += strconv.Itoa(2)
	fmt.Println(a)
    
    //使用strings.Builder拼接
    var build strings.Builder
	build.WriteString(a)
	build.WriteString(b)
	res := build.String()
	fmt.Println(res)
}

```

