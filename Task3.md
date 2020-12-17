# 运算符 and 控制语句
--------------
### 算数运算符
*  `+`   相加 
* `-`    相减
* `*`   相乘
* `/`   相除
* `%`  取余
* `++`  自增
* `--`  自减

```go
package main

func main() {
	a, b := 10, 20

	println("a+b:", a+b)
	println("a-b:", a-b)
	println("a*b:", a*b)
	println("a/b:", a/b)
	println("a%b:", a%b)
	a++
	println("a++:", a)
	b--
	println("b--:", b)

}
```

### 关系运算符
* `==` 相同
* `!=` 不同
* `>` 大于
* `<` 小于
* `>=` 大于等于
* `<=` 小于等于

```go
package main

import "fmt"

func main() {
	a, b := 20, 11

	if a == b {
		fmt.Println("a 等于 b")
	} else {
		fmt.Println("a 不等于b")
	}

}

```

### 逻辑运算符
* `&&` and运算符 
* `||` or运算符
* `!`   not运算符

```go
package main

import "fmt"

func main() {
	a, b := 20, 10

	if a == 20 && b == 10 {
		fmt.Println("The first: True")
	} else {
		fmt.Println("The first: False")
	}
	if a == 20 || b == 20 {
		fmt.Println("The sec: True")
	} else {
		fmt.Println("The sec: False")
	}
	if !(a == 20 && b == 10) {
		fmt.Println("The third: True")
	} else {
		fmt.Println("The third: False")
	}

}

```

### 位运算符
* `&` 按位与运算符"&"是双目运算符。 参与运算的两数各对应的二进位相与
* `|`按位或运算符"|"是双目运算符。 参与运算的两数各对应的二进位相或
* `^`按位异或运算符"^"是双目运算符
* `<<`左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方
* `>>`右移运算符">>"是双目运算符。右移n位就是除以2的n次方

```go
package main

import "fmt"

func main() {

	// a  1111
	// b 10110
	a, b := 15, 22
	// a&b 110
	fmt.Println(a & b)

	// a|b 11111
	fmt.Println(a | b)

	// a^b 11001
	fmt.Println(a ^ b)

	// a <<1 11110
	fmt.Println(a << 1)

	// b >> 1 1011
	fmt.Println(b >> 1)

}

```

### 赋值运算符
* `=` 赋值运算符
* `+=` 相加后再赋值
* `-=`相减后再赋值
* `*=`相乘后再赋值
* `/=`相除后再赋值
* `%=`求余后再赋值
* `<<=`左移后赋值
* `>>=`右移后赋值
* `%=`按位与后赋值
* `^=`	按位异或后赋值
* `!=`	按位或后赋值

```go
package main

import "fmt"

func main() {

	var a int = 10
	var b int
	b = a
	fmt.Println(b)
	// b= b*a 10*10
	b *= a
	fmt.Println(b)

	// b  = b %a 100 %10
	b %= a
	fmt.Println(b)

}

```

### 其他运算符
* `&` 返回变量存储地址
* `*`指针变量
```go
package main

func main() {
	var a int = 10
	println(&a)
}
```

### 优先级
* `5`  * / % << >> & &^
* `4`       + - | ^
* `3` == != < <= > >=
* `2` &&
* `1` ||

-------------------------
### 条件语句
#### if语句
* if 语句 由一个布尔表达式后紧跟一个或多个语句组成
* if 语句 后可以使用可选的 else 语句, else 语句中的表达式在布尔表达式为 false 时执行。
* if 或 else if 语句中可嵌入一个或多个 if 或 else if 语句。
* 同各类主流语言，不多赘述。但注意，Go 没有三目运算符，所以不支持 ?: 形式的条件判断

#### switch语句
* 用于基于不同条件执行不同动作，每一个 case 分支都是唯一的，从上至下逐一测试，直到匹配为止
* 匹配项后面不需要再加 break
* switch 默认情况下 case 最后自带 break 语句，匹配成功后就不会执行其他 case，如果我们需要执行后面的 case，可以使用 fallthrough 
* fallthrough:强制执行后面的 case 语句，fallthrough 不会判断下一条 case 的表达式结果是否为 true

```go
package main

import "fmt"

func main() {

	var a int = 10
	switch a {
	case 10:
		fmt.Println("True")
		// 不加fallthrough 输出TRUE 加了这个输出 True + False1
		fallthrough
	case 20:
		fmt.Println("False1")
	default:
		fmt.Println("False2")
	}
}

```

* 有点类似sql的case when语句


####  select语句
* 每个 case 都必须是一个通信
* 所有 channel 表达式都会被求值
* 所有被发送的表达式都会被求值
* 如果任意某个通信可以进行，它就执行，其他被忽略。
* 如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。 否则：
	* 如果有 default 子句，则执行该语句。
	* 如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。




### 循环语句
#### for循环语句
```go
package main

import "fmt"

func main() {
	var a int = 5
	for i := 0; i < a; i++ {
		fmt.Println(i)
	}

}
```

#### 循环嵌套
```go
package main

import "fmt"

func main() {
	var a int = 5
	for i := 0; i < a; i++ {
		for j := i; j < a; j++ {
			fmt.Println(i, j)
		}
	}

}
```


#### 循环控制语句
* break
	* 用于循环语句中跳出循环，并开始执行循环之后的语句
	* break 在 switch（开关语句）中在执行一条 case 后跳出语句的作用
	* 在多重循环中，可以用标号 label 标出想 break 的循环
* continue语句：跳过当前循环的剩余语句，然后继续进行下一轮循环
* goto：无条件转移到过程中指定行，与条件语句配合，实现条件转移、构成循环、跳出循环体等（不建议用，造成混乱）

```go
package main

import "fmt"

func main() {
	var a int = 5
	for i := 0; i < a; i++ {
		for j := i; j < a; j++ {

			if j == 3 {
			// continue 跳过本轮循环 break 跳出循环
				continue
			}
			fmt.Println(i, j)
		}
	}

}

```



--------------------------------
使用Go语言解决leetcode两数相加问题
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
nums = [2, 7, 11, 15], target = 9, nums[0] + nums[1] = 2 + 7 = 9,返回 [0, 1]

```go
// 暴力枚举
func twoSum(nums []int, target int) []int {
    n := len(nums)

    for i :=0; i <n;i++{
        for j:= i+1 ; j<n ;j++{
            if (nums[i] + nums[j] == target) {
                return [] int{i,j} 
            }
        }
    }
    return [] int{0,0}
}

```
