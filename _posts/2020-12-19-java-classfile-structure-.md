---
title: JVM 类文件结构
author: Kol Huang
date: 2020-12-19 15:50:00 +0800
categories: [Blogging, java]
tags: [jvm]
comments: true
math: true
---



Class文件是一组以8个字节为基础单位的二进制流，各个数据项严格按照顺序紧凑地排列在文件之中，中间没有添加任何分隔符。当遇到需要占用8个字节以上空间的数据项时，则会**以大端格式（Big-Endian）存储**（高位在前）。

根据《Java虚拟机规范》的规定，Class文件格式采用一种类似于C语言结构体的伪结构来存储数 据，这种伪结构中只有两种数据类型：“无符号数”和“表”。

无符号数属于基本的数据类型，以**u1、u2、u4、u8来分别代表1个字节、2个字节、4个字节和8个 字节的无符号数**，无符号数可以用来描述数字、索引引用、数量值或者按照UTF-8编码构成字符 值。

表是由多个无符号数或者其他表作为数据项构成的复合数据类型，为了便于区分，**所有表的命名都习惯性地以“_info”结尾。**表用于描述有层次关系的复合结构的数据，整个Class文件本质上也可以视作是一张表。

无论是无符号数还是表，当需要描述同一类型但数量不定的多个数据时，经常会使用一个**前置的容量计数器**加若干个连续的数据项的形式，这时候称这一系列连续的某一类型的数据为某一类型的“集合”。

Class的结构不像XML等描述语言，由于它没有任何分隔符号，所以所有数据项，无论是顺序还是数量，甚至于数据存储的字节序（Byte Ordering，Class 文件中字节序为Big-Endian）这样的细节，都是被严格限定的**，哪个字节代表什么含义，长度是多少， 先后顺序如何，全部都不允许改变。**

<img src="https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218085843611.png" alt="image-20201218085843611" style="zoom: 50%;" />

#### 魔数

**头4个字节是魔数：0xCAFEBABE**，用于确定这个文件是否为一个能被虚拟机接受的Class文件。



#### Class文件版本号

接下来**4个字节是Class文件的版本号**：第5和第6个字节是次版本号（Minor Version），第7和第8个字节是主版本号（Major Version）Java的版本号是从**45**开始的，JDK 1.1之后 的每个JDK大版本发布主版本号向上**加1**（**JDK 1.0～1.1使用了45.0～45.3的版本号**），高版本的JDK能 向下兼容以前版本的Class文件，但不能运行以后版本的Class文件

<img src="https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218090430886.png" alt="image-20201218090430886" style="zoom:50%;" />

次版本号，曾经在现代Java（即Java 2）出现前被短暂使用过，JDK 1.0.2支持的版本45.0～ 45.3（包括45.0～45.3）。JDK 1.1支持版本45.0～45.65535，从JDK 1.2以后，直到JDK 12之前次版本号均未使用，全部固定为零。而到了JDK 12时期，由于JDK提供的功能集已经非常庞大，**有一些复杂 的新特性需要以“公测”的形式放出，所以设计者重新启用了副版本号**，将它用于标识“技术预览版”功 能特性的支持。如果Class文件中使用了该版本JDK尚未列入正式特性清单中的预览功能，则必须把次版**本号标识为65535**，以便Java虚拟机在加载类文件时能够区分出来。



#### 常量池

**紧接着主、次版本号之后的是常量池入口**：由于常量池中常量的数量是不固定的，所以在常量池的入口需要放置一项u2类型的数据，代表常量池容量计数值（**constant_pool_count**），计数从**1**开始。例如：常量池容量（偏移地址：0x00000008）为十六进制数0x0016，即十进制**的22**，这就 代表常量池中有**21**项常量，索引值范围为**1～21**。设计者将**第0项常量** 空出来是有特殊考虑的，这样做的目的在于，如果后面某些指向常量池的索引值的数据在特定情况下 需要表达“**不引用任何一个常量池项目**”的含义，可以把索引值设置为0来表示。Class文件中只有常量池计数是从1开始，其他计数器都是从0计数。



常量池主要存放两大类常量：**字面量（Literal）**和**符号引用（Symbolic Refernces）**。字面量比较接近于于Java语言层面的常量概念，如文本字符串、被声明为final的常量值等。而符号引用则属于编译原理方面的概念，主要包括：

* 被模块导出或者开放的包（Package） 
* 类和接口的全限定名（Fully Qualified Name）
* 字段的名称和描述符（Descriptor）
* 方法的名称和描述符 
* 方法句柄和方法类型（Method Handle、Method Type、Invoke Dynamic）
* 动态调用点和动态常量（Dynamically-Computed Call Site、Dynamically-Computed Constant）

当虚拟机做类加载时，将会从常量池获得对应的符号引用，再在类创建时或运行时解析、翻译到具体的内存地址之中。

常量池中每一项常量都是一个表，最初常量表中共有11种结构各不相同的表结构数据，后来为了更好地支持动态语言调用，额外增加了4种动态语言相关的常量，为了支持Java模块化系统 （Jigsaw），又加入了CONSTANT_Module_info和CONSTANT_Package_info两个常量，所以截至JDK 13，常量表中分别有17种不同类型的常量。

![image-20201218091828646](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218091828646.png)

![image-20201218091806209](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218091806209.png)



![image-20201218091937387](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218091937387.png)

![image-20201218091954804](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218091954804.png)

![image-20201218092012816](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218092012816.png)

![image-20201218092025218](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218092025218.png)

由于Class文件中方法、字段等都需要引用CONSTANT_Utf8_info型常量来描述名 称，所以CONSTANT_Utf8_info型常量的最大长度也就是Java中方法、字段名的最大长度。而这里的 最大长度就是length的最大值，既u2类型能表达的最大值65535。所以Java程序中如果定义了超过64KB 英文字符的变量或方法名，即使规则和全部字符都是合法的，也会无法编译。



#### 访问标志

在常量池结束之后，紧接着的2个字节代表访问标志（access_flags），这个标志用于识别一些类或 者接口层次的访问信息

![image-20201218092405687](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218092405687.png)

access_flags中一共有16个标志位可以使用，当前只定义了其中9个，没有使用到的标志位要求一律为零。**上述所有标志位取或，得access_flags。**



#### 类索引、父类索引、接口索引集合

类索引（this_class）和父类索引（super_class）都是一个u2类型的数据，而接口索引集合 （interfaces）是一组u2类型的数据的集合，Class文件中由这三项数据来**确定该类型的继承关系。**

类索引用于确定**这个类的全限定名**，父类索引用于确定这个类的**父类的全限定名**。

接口索引集合就用来描述这个类实现了哪些接 口，这些被实现的接口将按implements关键字（如果这个Class文件表示的是一个接口，则应当是 extends关键字）后的**接口顺序从左到右排列在接口索引集合中**。

![image-20201218092739073](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218092739073.png)

对于接口索引集合，入口的第一项u2类型的数据为接口计数器（interfaces_count），表示索引表 的容量。





#### 字段表集合

字段表（field_info）用于描述**接口或者类中声明的变量**。Java语言中的“字段”（Field）包括类级变量以及实例级变量，但不包括在方法内部声明的局部变量。

字段可以包括的修饰符有字段的**作用域（public、private、protected修饰符**）、是实例变量还是类变量（**static修饰符**）、**可变性（final）**、**并发可见性（volatile修饰符，是否强制从主内存读写）**、**可否被序列化（transient修饰符**）、字段**数据类型（基本类型、对象、数组）**、**字段名称**。上述这些信息中，各个修饰符都是布尔值，要么有某个修饰符，要么没有，很适合使用标志位来表示。而字段叫做什么名字、字段被定义为什么数据类型，这些都是无法固定的，只能引用常量池中的常量来描述。

![image-20201218093323308](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218093323308.png)

字段修饰符放在**access_flags**项目中，是一个u2的数据类型。

![image-20201218093407350](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219095538074.png)

跟随access_flags标志的是两项索引值：**name_index**和**descriptor_index**。它们都是对常量池项的引用，分别代表着字段的**简单名称**以及字段和方法的**描述符**。简单名称指的是**没有类型和参数修饰**的方法或者字段名称。描述符的作用是用来描述字段的**数据类型**、**方法的参数列表**（包括**数量、类型以及顺序**）和**返回值**。

```markdown
org/fenixsoft/clazz/TestClass 这是一个全限定类名。
function	这是方法function(xxx)的简单名称
```

描述符：基本数据类型（byte、char、double、float、int、long、short、boolean）以及代表无返回值的void**类型都用一个大写字符来表示**，而对象类型则用**字符L加对象的全限定名**来表示

![image-20201218093840471](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201218093840471.png)

对于数组类型，每一维度将使用一个前置的“[”字符来描述，如一个定义为**“java.lang.String[][]”类型** 的二维数组将被记录成“**[[Ljava/lang/String；**”，一个整型数组“int[]”将被记录成“[I”。

用描述符来描述方法时，按照先参数列表、后返回值的顺序描述，参数列表按照参数的严格顺序放在一组小括号“()”之内。如方法`void inc()`的描述符为“`()V`”，方法`java.lang.String toString()`的描述符 为“`()Ljava/lang/String；`”，方法`int indexOf(char[]source，int sourceOffset，int sourceCount，char[]target， int targetOffset，int targetCount，int fromIndex)`的描述符为“`([CII[CIII)I`”。

字段表所包含的固定数据项目到descriptor_index为止就全部结束了，不过在descriptor_index之后 跟随着一个**属性表集合**，用于存储一些额外的信息，字段表可以在属性表中附加描述零至多项的额外 信息。

字段表集合中**不会列出从父类或者父接口中继承而来的字段**，但有**可能出现**原本Java代码之中**不存在的字段**，譬如在内部类中为了保持对外部类的访问性，编译器就会自动添加指向外部类实例的字段。

**在Java语言中字段是无法重载**的，两个字段的数据类型、修饰符不管是否相同，都必须使用不一样的名称，但是**对于Class文件格式来讲，只要两个字段的描述符不是完全相同，那字段重名就是合法的。**



#### 方法表集合

Class文件存储格式中对方法的描述与对字段的描述采用了几乎完全一致的方式，方法表的结构如同字段表一样，依次包括访问标志（access_flags）、名称索引（name_index）、描述符索引（descriptor_index）、属性表集合（attributes）几项。

![image-20201219095538074](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219095538074.png)

因为`volatile`关键字和`transient`关键字不能修饰方法，所以方法表的访问标志中没有了` ACC_VOLATILE`标志和`ACC_TRANSIENT`标志。与之相对，`synchronized`、`native`、`strictfp`和`abstract` 关键字可以修饰方法，方法表的访问标志中也相应地增加了`ACC_SYNCHRONIZED`、` ACC_NATIVE`、`ACC_STRICTFP`和`ACC_ABSTRACT`标志。

![image-20201219095700121](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219095700121.png)

方法的代码经过Javac编译器编译成字节码指令之后，存放在**方法属性表集合**中的一个名为"Code"的属性中。

如果父类方法在子类中没有被重写，方法表集合中就不会出现来自父类的方法信息。但有可能会出现编译器自动添加的方法，如`<clinit>()`和`<init>()`。

在Java语言中，要重载（Overload）一个方法，除了要与原方法具有相同的简单名称之外，还要求必须拥有一个与**原方法不同的特征签名** 。特征签名是指一个方法中**各个参数在常量池中的字段符号引用的集合**，也正是因为**返回值不会包含在特征签名之中**，所以Java语言里面是无法仅仅依靠返回值 的不同来对一个已有方法进行重载的。但是在Class文件格式之中，特征签名的范围明显要更大一些， 只要描述符不是完全一致的两个方法就可以共存。

```markdown
描述符：“`([CII[CIII)I`”
特征签名：I
返回值：([CII[CIII)
```



#### 属性表集合

Class文件、字段表、方法表都可以携带自己的属性表（attribute_info）集合，以描述某些场景专有的信息。

方法表集合之后的属性表集合，指的是class文件所携带的辅助信息，比如该class文件的源文件的名称，以及任何带有RetentionPolicy.CLASS或者RetentionPolicy.RUNTIME的注解。这类信息通常被用于Java虚拟机的验证与运行，以及Java程序的调试，一班无需深入了解。

与Class文件中其他的数据项目要求严格的顺序、长度和内容不同，属性表集合的限制稍微宽松一 些，**不再要求各个属性表具有严格顺序**，并且《Java虚拟机规范》允许只要不与已有属性名重复，任何人实现的编译器都可以向属性表中写入自己定义的属性信息，Java虚拟机运行时会忽略掉它不认识的属性。

对于每一个属性，它的名称都要从常量池中引用一个CONSTANT_Utf8_info类型的常量来表示， 而属性值的结构则是完全自定义的，只需要通过一个u4的长度属性去说明属性值所占用的位数即可。

一个符合规则的属性表应该满足

![image-20201219102930077](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219102930077.png)





##### Code属性

<img src="https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219104211632.png" alt="image-20201219104211632" style="zoom:50%;" />



code属性中，attribute_info表也可以有许多属性：

* LineNumberTable（字节码指令行号与Java代码行号的对应）；

  <img src="https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219110800803.png" alt="image-20201219110800803" style="zoom:50%;" />

* LocalVariableTable（局部变量表）

<img src="https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219110444297.png" alt="image-20201219110444297" style="zoom:50%;" />





##### SourceFile属性

<img src="https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201219142118481.png" alt="image-20201219142118481" style="zoom: 50%;" />

