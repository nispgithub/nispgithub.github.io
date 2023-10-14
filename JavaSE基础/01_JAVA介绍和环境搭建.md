



## 一、计算机基础知识

### 1.1 软件开发

​	什么是软件？

- 软件 （software）是一系列按照特定顺序组织的计算机**数据**和**指令**的集合

   软件的分类：

   - 系统软件（windows、linux、Android、ios）
   - 应用软件（QQ、微信、迅雷……）


什么是软件开发？

- 软件开发是根据用户的需求来建造出软件系统或者系统中的软件部分过程
- 软件一般是用**某种程序设计语言**来实现的

### 1.2 人机交互方式

#### 1.2.1 分类

- 图形用户界面方式（GUI:Graphical User Interface）：简单直观，易于接受，容易操作
- 命令行方式（CLI:Command Line Interface）：需要一个控制台，输入特定的指令，让计算机完成一些操作。方便快捷，专业使用，入门有难度，需要记住一些命令

#### 1.2.2 打开DOS窗口方式

- Win+R，然后输入cmd，确定
- 开始->windows系统->命令提示符
- 进入文件夹，在地址栏输入cmd

#### 1.2.3 常用DOS命令

| 命令 | 功能                                                         |
| ---- | ------------------------------------------------------------ |
| dir  | 列出当前目录下的文件以及目录                                 |
| md   | 创建目录                                                     |
| rd   | 删除目录<br />/s除目录本身外，还将删除指定目录下的所有子目录和文件。用于删除目录树。<br />/q安静模式，带/q删除目录树是不需要确认 |
| cd   | 进入指定目录<br />.当前目录<br />..上一级目录<br />\根目录   |
| del  | 删除文件                                                     |
| exit | 退出dos命令行                                                |
| cls  | 清屏                                                         |

常用快捷键：

- tab：自动补全
- ← →：移动光标
- ↑ ↓：调阅历史操作命令
- Delete和Backspace：删除字符

#### 1.3 练习

常用dos命令

### 二、Java语言概述

#### 2.1 Java语言简介

**语言**：人与人交流的一种工具。例如：汉语、英语。

**计算机语言**：人与计算机交流的一种语言。要与计算机交流，就要学习计算机可以理解的语言。

**Java语言**

推出公司：Sun Microsystems（Sun公司），后被Oracle收购。

主要作者：James Gosling（詹姆斯·高斯林）和同事。

历史：

* 1991年为消费类电子产品研发，最初名为Oak；
* **1995年正式推出，更名为Java；**
* 1996年，发布JDK 1.0，约8.3万个网页应用Java技术来制作；
* 1997年，发布JDK 1.1，JavaOne会议召开，创当时全球同类会议规模之最；
* 1998年，发布JDK 1.2，同年发布企业平台J2EE；
* 1999年，Java分成J2SE、J2EE和J2ME，JSP/Servlet技术诞生；
* **2004年，发布里程碑式版本：JDK 1.5，为突出此版本的重要性，更名为JDK 5.0；**
* 2005年，J2SE -> JavaSE，J2EE -> JavaEE，J2ME -> JavaME；
* 2009年，Oracle公司收购SUN，交易价格74亿美；
* 2011年，发布JDK 7.0
* **2014年，发布JDK8.0，是继JDK 5.0以来变化最大的版本；**
* ...
* **2021年，发布JDK17**
* ...

#### 2.2  Java语言平台版本

**JavaSE（Java Standard Edition）标准版（本阶段需要学习）**

* 支持面向桌面级应用（如Windows下的应用程序）的Java平台，提供了完整的Java核心API，此版本以前称为J2SE。

**JavaEE（Java Enterprise Edition）企业版（要深入学习的内容）**

* 为开发企业环境下的应用程序提供的一套解决方案。该技术体系中包含的技术如：Servlet 、Jsp等，主要针对于Web应用程序开发。版本以前称为J2EE。

**JavaME（Java Micro Edition）小型版（不需要学习）**

* 支持Java程序运行在移动终端（手机、PDA）上的平台，对Java API有所精简，并加入了针对移动终端的支持，此版本以前称为J2ME

#### 2.3 Java语言特点

- 简单的
- 面向对象
- 健壮的
- 跨平台的
- 高性能的
- 分布式的

#### 2.4 Java应用领域

**企业级应用**：主要指复杂的大企业的软件系统、各种类型的网站。Java的安全机制以及 它的跨平台的优势，使它在分布式系统领域开发中有广泛应用。应用领域包括金融、电 信、交通、电子商务等。

**Android平台应用**：Android应用程序使用Java语言编写。Android开发水平的高低很大程度上取决于Java语言核心能力是否扎实。

**大数据平台开发**：各类框架有Hadoop，spark，storm，flink等，就这类技术生态 圈来讲，还有各种中间件如flume，kafka等 ，这些框架以及工具大多数 是用Java编写而成，但提供诸如Java，scala，Python，R等各种语言API供编程。

#### 2.5 为什么学习Java

上面的特点及应用领域；

市场占有率：

* TIOBE排行榜长期占据前三的位置；
* 国内代码托管平台Gitee，Java开源项目占有率长期处于第一的位置

![](.\pic\TIOBE排行.jpg)

![](.\pic\Gitee编程语言排行.jpg)

### 三、Java开发环境搭建

#### 3.1 环境搭建

​	严格按照教程，一步步完成安装

1. **下载**

   `https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html`

   注意：下载时选择正确的版本！！！

   ![](.\pic\JDK版本.jpg)

2. **安装JDK**

   单击“下一步”

   ![](.\pic\01_jdk安装.jpg)

   注意选择JDK安装路径，一定要安装在一个**没有中文和特殊符号**的路径下。

   <img src=".\pic\02_jdk安装.jpg " />

   安装图示的要求，选择**不安装JRE**，为什么不安装，后面会解释。

   ![](.\pic\03_jdk安装.jpg)

   单击“关闭”完成安装。

   ![](.\pic\04_jdk安装.jpg)

   

3. **配置环境变量**

   此电脑 - 右键 - 属性 - 高级系统设置 - 环境变量，打开环境变量窗口

   新建`JAVA_HOME`环境变量，变量值填写**jdk安装目录的bin的上一级目录**。

   

   ![](.\pic\05_配置环境变量.jpg)

   配置`Path`环境变量（%JAVA_HOME%\bin）

   ![](.\pic\06_配置环境变量.jpg)

   

4. **验证安装配置是否成功**

   **重新打开**一个CMD窗口，分别输入下面两条命令，如果**有版本信息输出**并且**版本值相同**证明JDK安装配置成功。

   ```sh
   > javac -version
   > java -version
   ```

   

#### 3.2 需要明确的几个问题

1. **为什么配置环境变量**？

   配置环境变量的目的是为了方便在控制台编译和运行java程序，不必进入到Java的程序目录里去运行。桌面上的Java程序文件也可以编译运行。

   Windows系统查找过程：

   当前目录 ->Path环境变量指定的目录

2. **JDK、JRE、JVM的关系**？

   **JDK**： Java Development Kit，Java开发包，是Sun提供的一套用于开发Java应用程序的开发包，它提供了编译、运行 Java 程序所需要的各种工具和资源，包括 Java 编译器、Java 运行时环境（JRE），以及常用的Java类库等。

   **JRE**：Java Runtime Environment，Java 运行时环境，它是运行 Java 程序的必要条件。

   **JVM**：Java Virtual Machine，Java虚拟机，JVM是可以运行Java字节码文件的虚拟计算机。oracle不提供JVM的下载。

   ![](.\pic\image关系图.png)

   安装JDK是选择“公共JRE”不可用，是因为JDK已经包含JRE!

3. **Java的跨平台是如何实现的**？

   Java的跨平台性：通过Java语言编写的应用程序在不同的系统平台上都可以运行（一次编译，到处运行）。

   如何实现：

   ![](.\pic\Java的跨平台性.jpg)

   只要在需要运行Java应用程序的操作系统上，先安装一个Java虚拟机(JVM Java Virtual Machine)即可。

   

### 四 第一个Java程序

#### 4.1 开发

> 新建一个`txt`文件，将文件名修改为`HelloWorld.java`。
>
> 注意：一定要设置让Windows操作系统显示文件的扩展名。
>
> 使用文本编辑工具编辑这个文件，输入如下的内容，并**保存**。

```java
//在控制台打印Hello World！	
//定义一个类，注意大括号是成对的
public class HelloWorld {
    //类中的内容都要写在大括号内
    //定义一个main方法，main方法是整个程序的入口
	public static void main(String[] args) {
        //在控制台输出内容，注意双引号，分号，输入法
		System.out.println("Hello World!");
	}
}
```

> 由于我们刚刚接触Java，上面案例的很多内容我们暂时无法深入展开，大家先把这段程序**背下来**。

#### 4.2、编译

> 打开`cmd`，进入上面文件所在的文件夹，输入如下的命令：

```sh
javac HelloWorld.java
```

> 这一步的作用是编译Java程序，我们能够看到文件夹下生成了一个`.class`文件。
>
> 编译通俗的说就是将人们能够看懂的代码（上面的`.java`文件）转换成计算机能够识别的代码（生成的`.class`文件）。

#### 4.3、运行

> 执行如下的命令：

```sh
java HelloWorld
```



注意：一个java文件可以有多个类（class），但只能有一个被public修饰的类！

### 五 输入输出

​	

```java
//在控制台打印输入内容！
import java.util.Scanner;  
//定义一个类，注意大括号是成对的
public class HelloWorld2 {
    //类中的内容都要写在大括号内
    //定义一个main方法，main方法是整个程序的入口
	public static void main(String[] args) {
        //console: 控制台 Scanner：扫描器
		// Scanner 可以用于从控制台读取信息
		Scanner console = new Scanner(System.in);
        System.out.print("请输入你的名字：");
		//console.nextLine() 读取“控制台的下一行”文本
		String name = console.nextLine();//调用API的功能
		System.out.println("Java欢迎你(^_^)!"+name);

	}
}
```

```sh
编译:
javac -encoding utf-8 HelloWorld2.java
运行：
java HelloWorld2

```







