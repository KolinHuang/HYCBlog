---
title: JVM in Go Style
author: Kol Huang
date: 2021-01-04 20:50:00 +0800
categories: [Blogging, java]
tags: [jvm]
math: true

---



目录：

[1 命令行工具](#jump1)

[1.1 准备工作](#jump1.1)

[1.2 Java命令](#jump1.2)

[1.3 编写命令行工具](#jump1.3)

[1.4 测试命令行工具](#jump1.4)



[2 搜索class文件](#jump2)

<span id = "jump1"></span>

## 1 命令行工具

<span id = "jump1.1"></span>

### 1.1 准备工作

**操作系统**：macOS 10.15.7 

**java version**： "1.8.0_201"

**go version**： go1.15.3 darwin/amd64

#### 1.1.1/2 安装JDK和Go

省略了安装的步骤。

go 命令行希望所有的Go源代码都被放在一个工作空间中。所谓工作空间，实际上就是一个目录结构，这个目录结构包含三个子目录。

* .src目录中是Go语言的源代码
* .pkg目录中是编译好的包对象文件
* .bin目录是链接好的可执行文件

只有src目录是我们需要写的，go会自动创建其余两个目录，工作空间可以位于任何地方，本实现（Mac环境下）的工作空间为:

```shell
/Users/huangyucai/go
```

如果要自定义工作空间，可使用以下命令：

```shell
#查看目前的环境
go env

vim ~/.bash_profile
#修改GPPATH项
GOPATH=/xxxxxx
#使其生效
source ~/.bash_profile
#查看效果
go env
```

#### 1.1.3 创建目录结构

Go语言以包为单位组织源代码，包可以嵌套，形成层次关系。本实现的所有源代码放在jvmgo包中，所以首先创建目录如下

![image-20210104193557762](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210104193557762.png)





<span id = "jump1.2"></span>

### 1.2 Java命令

Java程序有一个主类用来启动Java应用，此类中包含一个main()方法。Java虚拟机规范没有明确规定JVM如何获取和启动主类，所以应由虚拟机实现自行决定启动方式。Oracle的Java虚拟机实现使用Java命令来启动，主类名在命令行参数中指定。Java命令有以下4种形式：

```shell
java [-options] class [args]
java [-options] -jar jarfile [args]
javaw [-options] class [args]
javaw [-options] -jar jarfile [args]
```

通常，第一个非选项参数给出主类的完全限定名，但是如果用户指定了-jar参数，那第一个非选项参数表示JAR文件名，Java命令必须从这个JAR文件中寻找主类。javaw命令和java命令几乎完全一样，唯一的差别在于，javaw命令不显示命令行窗口，因此特别适合用于启动GUI应用程序。

**常用选项**

![image-20210104194820326](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210104194820326.png)





<span id = "jump1.3"></span>

### 1.3 编写命令行工具

在chap01目录下创建文件`cmd.go`，并编写结构体代码：

```go
type Cmd struct {
	helpFlag	bool//用于指定是否显示帮助信息
	versionFlag	bool//用于指定是否显示版本信息
	clspath	string//用于指定类路径
	class		string//用于指定主类
	args		[]string//用于指定其他参数
}
```

再编写一个用于解析命令行参数的函数，在Go中可以直接处理os.Args变量来完成以上需求，但是比较麻烦。Go还内置了flag包可以帮助我们优雅地处理命令行选项。[API地址](https://golang.google.cn/pkg/flag/)

常用的几个方法：

```go
//声明一个flag: "-n"，存储在类型为*int的指针nFlag中
import "flag"
var nFlag = flag.Int("n", 1234, "help message for flag n")
//还可以用Var方法将一个flag绑定到一个变量
var flagvar int
func init() {
	flag.IntVar(&flagvar, "flagname", 1234, "help message for flagname")
}

//定义完flag后，调用以下方法将命令行参数解析到上面定义好的flags中
flag.Parse()
```

本实现中，处理命令行的函数

```go
func parseCmd() *Cmd {
	//创建一个Cmd结构体对象
	cmd := &Cmd{}
	//如果Parse函数解析失败，就会调用printUsage函数把命令的用法打印到控制台
	flag.Usage = printUsage
	//将help和？这两个参数绑定到变量&cmd.helpFlag，初值为false，信息为print help message
	//这样如果在命令行中出现help和?参数，说明需要打印帮助信息
	flag.BoolVar(&cmd.helpFlag, "help", false, "print help message-打印帮助信息")
	flag.BoolVar(&cmd.helpFlag, "? ", false, "print help message-打印帮助信息")
	flag.BoolVar(&cmd.versionFlag, "version", false, "print version and exit-打印命令版本")
	flag.StringVar(&cmd.clspath, "classpath", "", "classpath-指定类文件路径")
	flag.StringVar(&cmd.clspath, "cp", "", "classpath-指定类文件路径")
	//当所有flag定义完毕，调用此方法来解析命令行参数到flags中
	flag.Parse()
	args := flag.Args()
	if len(args) > 0 {
		cmd.class = args[0]
		cmd.args = args[1: ]
	}
	return cmd
}

func printUsage() {
	fmt.Println("Usage:")
	fmt.Printf("%s [-options] class [args...]\n", os.Args[0])
	fmt.Println("Options:")
	flag.PrintDefaults()
}
```

printUsage函数会在flag.Parse解析失败后被调用，显示命令的用法，如果解析成功，命令行参数将被解析到结构体对应的变量中。





<span id = "jump1.4"></span>

### 1.4 测试命令行工具

在chap01目录下编写main.go文件：

```go
package main
import "fmt"

func main() {
	cmd := parseCmd()
	if cmd.versionFlag {
		fmt.Println("version: 0.0.1")
	} else if cmd.helpFlag || cmd.class == ""{
		//用户指定了helpFlag参数或者未指定主类，就打印命令用法
		printUsage()
	} else {
		//一切正常就启动Java虚拟机
		startJVM(cmd)
	}
}

func startJVM(cmd *Cmd){
  fmt.Println("JVM start running...")
	fmt.Printf("classpath: %s class: %s args: %v\n",
		cmd.clspath, cmd.class, cmd.args)
}
```

测试命令`chap01 -help`，输出如下所示：

![image-20210104204359043](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210104204359043.png)

测试命令`chap01 -cp java/classpath MyApp arg1 arg2 arg3`

![image-20210104204610033](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210104204610033.png)





<span id = "jump2"></span>

## 2 搜索class文件







