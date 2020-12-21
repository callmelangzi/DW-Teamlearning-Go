### 包管理
------------------------
#### Go Modules
* Go语言通过包管理来封装模块和复用代码
* Go Modules于Go语言1.11版本时引入，在1.12版本正式支持，是由Go语言官方提供的包管理解决方案
* Modules是相关Go包的集合，是源代码交换和版本控制的单元。go命令直接支持使用Modules，包括记录和解析对其他模块的依赖性

#### Go Modules的使用方法
##### 环境变量
可以使用go env命令查看当前配置,如果需要更改 GO111MODULE ，可以使用go env命令
	
	$ go env 
	go env -w GO111MODULE=on

`GO111MODULE`
* auto：只要项目包含了 go.mod 文件的话启用 Go modules，目前在 Go1.11 至 Go1.14 中仍然是默认值。
* on：启用 Go modules，推荐设置，将会是未来版本中的默认值。
* off：禁用 Go modules，不推荐设置。
`GOPROXY`
此环境变量主要用于设计Go Module的代理( 在初探Go中使用过-用来下载vscode插件)

`GOSUMDB`
此环境变量用于在拉取模块的时候保证模块版本数据的一致性。

#### 初始化模块
Go Modules的使用方法比较灵活，在目录下包含go.mod文件即可

	go mod init [module name]
然后当前目录会生成go.mod文件，其内容为：

	module ModuleName
	go 1.15
Go Modules会自动管理包，如果需要引入依赖，只需要在go.mod下添加以下内容（以gorose为例子）

	module ModuleName
	 
	require (
		github.com/gohouse/gorose v1.0.5
	)
Go Modules会自动管理包，如果需要引入依赖，只需要在go.mod下添加以下内容

	module ModuleName
 
	require (
	github.com/gohouse/gorose v1.0.5
	)

##### go get 

go get 命令用于拉取新的依赖，以下为go get命令具体用法
* `go get` 拉取依赖，会进行指定性拉取（更新），并不会更新所依赖的其它模块
* `go get -u` 更新现有的依赖，会强制更新它所依赖的其它全部模块，不包括自身。
* `go get -u -t ./...` 更新所有直接依赖和间接依赖的模块版本，包括单元测试中用到的。



其他参数

		
		-d 只下载不安装
		-f 只有在你包含了 -u 参数的时候才有效，不让 -u 去验证 import 中的每一个都已经获取了，这对于本地 fork 的包特别有用
		-fix 在获取源码之后先运行 fix，然后再去做其他的事情
		-t 同时也下载需要为运行测试所需要的包
		-u 强制使用网络去更新包和它的依赖包
		-v 显示执行的命令


#####  常用命令

		go mod init  // 初始化go.mod
		go mod tidy  // 更新依赖文件
		go mod download  // 下载依赖文件
		go mod vendor  // 将依赖转移至本地的vendor文件
		go mod edit  // 手动修改依赖文件
		go mod graph  // 查看现有的依赖结构
		go mod verify  // 校验依赖
