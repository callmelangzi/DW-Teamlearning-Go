### 单元测试
-----------------
在日常开发中，我们通常需要针对现有的功能进行单元测试，以验证开发的正确性。 在go标准库中有一个叫做testing的测试框架，可以进行单元测试，命令是go test xxx。

测试文件通常是以xx_test.go命名，放在同一包下面。

#### 初探Go单元测试
在开发中，我们需要对该函数进行功能测试，如何快速进行单元测试呢？
鼠标放在函数上右键，选择GO:Generate Unit Tests For Function即可生成file_test.go文件。在命令行运行

	go test ***_test.go  ***.go -v -cover

输出为

	=== RUN   TestAdd
	--- PASS: TestAdd (0.00s)
	PASS
	coverage: 0.0% of statements
	ok      command-line-arguments  0.044s  coverage: 0.0% of statements


####  单测要点
单元测试的时候，如果有一些打印log信息，我们运行xxx_test.go是输出不出来的，此时需要使用

	go test xxx_test.go -v
单测覆盖率，覆盖率可以简单理解为进行单元测试mock的时候，能够覆盖的代码行数占总代码行数的比率，当然是高一点要好些。可以通过-cover指定

	go test xxx_test -v -cover

在上述提到的测试方法中我们使用的是(table-driven tests)表格驱动型测试
```go
package utest

import (
	"fmt"
	"reflect"
	"testing"
)

func TestAdd(t *testing.T) {
	type args struct {
		a Complex
		b Complex
	}
	tests := []struct {
		name string
		args args
		want *Complex
	}{
		// TODO: Add test cases.
		{
			name: "",
			args: args{
				a: Complex{
					Real: 1.0,
					Imag: 2.0,
				},
				b: Complex{
					Real: 1.0,
					Imag: 1.0,
				},
			},
			want: &Complex{
				Real: 2.0,
				Imag: 3.0,
			},
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := Add(tt.args.a, tt.args.b); !reflect.DeepEqual(got, tt.want) {
				t.Errorf("Add() = %v, want %v", got, tt.want)
			}
		})
	}
}

func BenchmarkComplex(t *testing.B) {

	for i := 0; i < t.N; i++ {
		fmt.Sprintf("hello")
	}
}



```

* 基准测试函数名字必须以Benchmark开头


#### mock/stub测试
gomock是官方提供的mock框架，同时有mockgen工具来辅助生成测试代码
https://github.com/golang/mock
需要自己先安装一下,命令行输入

	go get -u github.com/golang/mock/gomock
	go get -u github.com/golang/mock/mockgen

后面一点跳过了···看不明白了

#### 直接替换
命令行安装失败了···

	go get github.com/bouk/monkey

##### 浏览器实时测试
比较方便的单元测试框架，可以在浏览器进行实时查看单元测试结果
只需要三步即可
第一步:

	go get github.com/smartystreets/goconvey

第二步：

	$GOPATH/bin/goconvey ($GOPATH 替换成自己电脑上配置可通过 go env 命令看)
	
	C:/Users/Lenovo/go/bin/goconvey

第三步:

	浏览器输入  http://localhost:8080/

