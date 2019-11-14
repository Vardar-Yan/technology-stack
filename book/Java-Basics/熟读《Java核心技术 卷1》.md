# 熟读《Java核心技术 卷1》

<a name="OGUfp"></a>
## 前言


---

<a name="G7AEb"></a>
## 书名


---


<a name="S6XyL"></a>
## 资源下载


---

<a name="ajJ4W"></a>
## 第1章 Java程序设计概述


<a name="iQIpz"></a>
## 第2章 Java程序设计环境
<a name="oO2PQ"></a>
## 第3章 Java的基本程序设计结构

<a name="OJxHp"></a>
### 3.1 一个简单的Java应用程序
<a name="Fdrg7"></a>
### 3.2 注释
<a name="IJnC2"></a>
### 3.3 数据类型

Java是一种强类型语言。这就意味着必须为每一个变量声明一种类型。在Java中，一共有8种基本类型（primitive type），其中有4种整型、2种浮点类型、1种用于表示Unicode编码的字符单元的字符类型char（请参见论述**char**类型的章节）和1种用于表示其值得**boolean**类型。

注释：Java有一个能够表示任意精度的算术包，通常称为“大数值”（big number）。虽然被称为大数值，但它并不是一种新的Java类型，而是一个Java对象。本章稍后将会详细地介绍它的用法。<br />

<a name="d3Aqc"></a>
#### 3.3.1 整型

整型用于表示没有小数部分的数值，它允许是负数。Java提供了4种整型，具体内容如表3-1所示。<br />
<br />**表3-1 Java整型**

| **类型** | **存储需求** | **取值范围** |
| :---: | :---: | :---: |
| **int** | **4字节** | -2 147 483 648 - 2 147 483 647 (正好超过 20 亿) |
| **short** | **2字节** | -32 768 - 32 767 
<br /> |
| **long** | **8字节** | -9 223 372 036 854 775 B08 - 9 223 372 036 854 775 807 
<br /> |
| **byte** | **1字节** | -128 ~ 127 |



<a name="ac4zX"></a>
#### 3.3.2 浮点类型

浮点类型用于表示有小数部分的数值。在Java中有两种浮点类型，具体内容如表3-2所示。<br />
<br />**表3-2 浮点类型**

| **类型** | **存储需求** | **取值范围** |
| :---: | :---: | :---: |
| **float** | **4字节** | 大约 ± 3.402 823 47E+38F (有效位数为 6 ~ 7 位） |
| **double** | **8字节** | 大约 ± 1.797 693 134 862 315 70E+308 (有效位数为 15 位> |


double表示这种类型的数值精度是float类型的两倍（有人称之为双精度数值）。绝大部分应用程序都采用double类型。在很多情况下，float类型的精度很难满足需求。实际上，只有很少的情况适合使用float类型，例如，需要单精度数据的库，或者需要存储大量数据。<br />float类型的数值有一个后缀F或f（例如，3.14F）。没有后缀F的浮点数值（如3.14）默认为double类型。当然，也可以在浮点数值后面添加后缀D或d（例如，3.14D）。

<a name="0P9bS"></a>
#### 3.3.3 char类型

char 类型原本用于表示单个字符。不过，现在情况已经有所变化。如今，有些 Unicode 字符可以用一个 char 值描述，另外一些 Unicode 字符则需要两个 char 值。有关的详细信息请阅读下一节。

<a name="yy1VT"></a>
#### 3.3.4 Unicode和char类型

<a name="YNZmT"></a>
#### 3.3.5 boolean类型
**boolean** （布尔）类型有两个值：**false** 和 **true** ，用来判定逻辑条件。整型值和布尔值之间不能进行相互转换。

<a name="5GaUN"></a>
### 3.4 变量
<a name="4pfxN"></a>
### 3.5 运算符
<a name="U5bow"></a>
### 3.6 字符串
<a name="vCrin"></a>
### 3.7 输入输出
<a name="6iIBT"></a>
### 3.8 控制流程
<a name="Wq7kI"></a>
### 3.9 大数值
<a name="78VI0"></a>
### 3.10 数组

<a name="bk42D"></a>
## 第4章 对象与类

<a name="EZSmb"></a>
### 4.1 面向对象程序设计概述

<a name="FM8nN"></a>
#### 4.1.1 类
类（class）是构造对象的模板或蓝图。由类构造**（construct）**对象的过程称为创建类的实例**（instance）**。

<a name="ZBuKl"></a>
#### 4.1.2 对象

要想使用OOP，一定要清楚对象的三个主要特性：

- **对象的行为（behavior）—— 可以对对象施加哪些操作，或可以对对象施加哪些方法？**
- **对象的状态（state）—— 当施加那些方法时，对象如何响应？**
- **对象标识（identity）—— 如何辨别具有相同行为与状态的不同对象？**



<a name="hmNFD"></a>
#### 4.1.3 识别类
<a name="sh4fu"></a>
#### 4.1.4 类之间的关系

在类之间，最常见的关系有

- **依赖（“uses-a”）**
- **聚合（“has-a”）**
- **继承（“is-a”）**

依赖（dependence），即“uses-a”关系，是一种最明显的、最常见的关系。例如，Order 类使用 Account 类是因为 Order 对象需要访问 Account 对象查看信用状态。但是 Item 类不依赖于 Account 类，这是因为 Item 对象与客户账户无关。因此，如果一个类的方法操纵另一个类的对象，我们就说一个类依赖于另一个类。<br />
<br />应该尽可能地将相互依赖的类减至最少。如果类 A 不知道 B 的存在，它就不会关心 B 的任何改变（这意味着 B 的改变不会导致 A 产生任何 bug）。用软件工程的术语来说，就是让类之间的耦合度最小。<br />


| **关系** | **UML连接符** |
| :---: | :---: |
| 继承 | ![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572691606936-5aea6e29-7ae2-4e13-97e4-6ce4bebc70dd.png#align=left&display=inline&height=23&name=image.png&originHeight=21&originWidth=225&search=&size=2344&status=done&width=247) |
| 接口实现 | ![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572691549004-8e8c388d-7158-438a-b0b9-62c6246f5fdc.png#align=left&display=inline&height=34&name=image.png&originHeight=34&originWidth=223&search=&size=2894&status=done&width=222) |
| 依赖 | ![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572691634585-acb13903-bdfa-44a4-97bd-5d78a0a69178.png#align=left&display=inline&height=23&name=image.png&originHeight=25&originWidth=227&search=&size=2212&status=done&width=210) |
| 聚合 | ![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572691659575-4e92eb71-6f9c-42f1-be60-e19a18537b99.png#align=left&display=inline&height=24&name=image.png&originHeight=30&originWidth=222&search=&size=2248&status=done&width=178) |
| 关联 | ![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572691677142-74d3d472-e459-4d13-990e-983620bbfec6.png#align=left&display=inline&height=24&name=image.png&originHeight=32&originWidth=233&search=&size=1611&status=done&width=175) |
| 直接关联 | ![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572691699444-0d14c46e-66cb-4e46-b154-78956f56a7c4.png#align=left&display=inline&height=24&name=image.png&originHeight=20&originWidth=227&search=&size=2178&status=done&width=274) |





<a name="7CsLG"></a>
### 4.2 使用预定义类
<a name="f4lxQ"></a>
### 4.3 用户自定义类
<a name="WiKYm"></a>
### 4.4 静态域与静态方法

<br />在前面给出的实例程序中，main方法都被标记为 static 修饰符。下面讨论一下这个修饰符的含义。<br />

<a name="CQkvy"></a>
#### 4.4.1 静态域
如果将域定义为 static，每个类中只有一个这样的域。而每一个对象对于所有的实例域却都有自己的一份拷贝。例如，假定需要给每一个雇员赋予唯一的标识码。这里给 Employee 类添加一个实例域 id 和 一个静态域 nextId:

```java
class Employee{
	private static int nextId = 1;
    private int id;
    ...
}
```



<a name="Oi7NJ"></a>
#### 4.4.2 静态常量


<a name="wEnfW"></a>
#### 4.4.3 静态方法
静态方法是一种不能向对象实施操作的方法。例如，Math 类的 pow 方法就是一个静态方法。表达式**Math.pow(x,a)**<br />计算幂 $$X^a
$$ 。在运算时，不使用任何 Math 对象。换句话说，没有隐式的参数。

<a name="XKK39"></a>
#### 4.4.4 工厂方法
<a name="tBON3"></a>
#### 4.4.5 main方法

<a name="rJP8S"></a>
### 4.5 方法参数
<a name="g9xsm"></a>
### 4.6 对象构造
<a name="HVKbi"></a>
### 4.7 包
<a name="9QBtx"></a>
### 4.8 类路径
<a name="wddn0"></a>
### 4.9 文档注释
<a name="IJSGC"></a>
### 4.10 类设计技巧


<a name="kJql0"></a>
## 第5章 继承

<a name="AFyV9"></a>
### 5.1 类、超类和子类
<a name="jxU40"></a>
### 5.2 Object：所有类的超类
<a name="1FWnw"></a>
### 5.3 泛型数组列表
<a name="5wwio"></a>
### 5.4 对象包装器与自动装箱
<a name="jNOQh"></a>
### 5.5 参数数量可变的方法
<a name="3Z5i1"></a>
### 5.6 枚举类
<a name="tuPow"></a>
### 5.7 反射

反射库（reflection library）提供了一个非常丰富且进行设计的工具集，以便编写能够动态操纵Java代码的程序。这项功能被大量应用于JavaBeans中，它是Java组件的体系结构。使用反射，Java可以支持Visual Basic用户习惯使用的工具。特别是在色剂或运行中添加新类时，能够快速地应用开发工具动态地查询新添加类的能力。<br />能够分析类能力的程序称为反射（reflective）。反射机制的功能极其强大，在下面可以看到，反射机制可以用来：

- 在运行时分析类的能力。
- 在运行时查看对象，例如，编写一个toString方法供所有类使用。
- 实现通用的数组操作代码。
- 利用Method对象，这个对象很想C++中的函数指针。

反射是一种功能强大且复杂的机制。使用它的主要人员是工具构造者，而不是应用程序员。如果仅对设计应用程序感兴趣，而对构造工具不感兴趣，可以跳过本章的剩余部分，稍后再返回来学习。<br />

<a name="gdQfj"></a>
#### 5.7.1 Class类

在程序运行期间，Java运行时系统始终为所有的对象维护一个被称为运行时的类型标识。这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。<br />
<br />然而，可以通过专门的Java类访问这些信息。保存这些信息的类被称为Class，这个名字很容易让人混淆。Object类中的getClass()方法将会返回一个Class类型的实例。<br />

```java
Employee e;
...
Class cl = e.getClass();
```

如同用一个Employee对象表示一个特定的雇员属性一样，一个Class对象将表示一个特定类的属性。最常用的Class方法是getName。这个方法将返回类的名字。例如，下面这条语句：

```java
System.out.println(e.getClass().getName() + "" + e.getName())
```

如果e是一个雇员，则会打印输出：

```java
Employee Harry Hacker
```

<br />如果e是经理，则会打印输出：

```java
Manager Harry Hacker
```

<br />如果类在一个包里，包的名字也作为类名的一部分：

```java
Random generator = new Random();
Class cl = generator.getClass();
String name = cl.getName();//name is set to "Java.util.Random"
```

<br />还可以调用静态方法forName获得类名对应的Class对象。

```java
String className = "java.util.Random";
Class cl = Class.forName(className);
```

<br />如果类名保存在字符串中，并可在运行中改变，就可以使用这个方法。当然，这个方法只有在className是类名或接口名时才能够执行。否则，forName方法将抛出一个checked exception（已检查异常）。无论何时使用这个方法，都应该提供一个异常处理器（exception handler）。如何提供一个异常处理器，请参看下一节。<br />
<br />提示：在启动时，包含main方法的类被加载。它会加载所有需要的类。这些被加载的类又要加载它们需要的类，一次类推。对于一个大型的应用程序来说，这将会消耗很多时间，用户会因此感到不耐烦。可以使用下面这个技巧给用户一种启动速度比较快的幻觉。不过，要确保包含main方法的类没有显式地引用其他的类。首先，显式一个启动画面；然后，通过调用Class.forName手工地加载其他的类。<br />
<br />获得Class类对象的第三种方法非常简单。如果T是任意的Java类型（或void关键字），T.class将代表匹配的类对象。例如：

```java
Class cl1 = Random.class;//if you import java.util.*;
Class cl2 = int.class;
Class cl3 = Double[].class;
```

<br />请注意，一个Class对象实际上表示的是一个类型，而这个类型未必一定是一种类。例如，int不是类，但是int.class是一个Class类型的对象。

**注释：Class类实际上是一个泛型类。例如，Employee.class的类型是Class<Employee>。没有说明这个问题的原因是：它将已经抽象的概念更加复杂化了。在大多数实际问题中，可以忽略类型参数，而使用原始的Class类。有关这个问题更详细的论述请参看第8章。**

**警告：鉴于历史原因，getName方法在应用于数组类型的时候会返回一个很奇怪的名字：**

- **Double[].class.getName() 返回 “[Ljava.lang.Double;]”**
- **int[].class.getName 返回 “[I”。**

虚拟机为每个类型管理一个Class对象。因此，可以利用 == 运算符实现两个类对象比较的操作。例如，

```java
if(e.getClass() == Employee.class)...
```

还有一个很有用的方法newInstance()，可以用来动态的创建一个类的实例。例如，

```java
e.getClass().newInstance();
```

创建了一个与e具有相同类类型的实例。newInstance方法调用默认的构造器（没有参数的构造器）初始化新创建的对象。如果这个类没有默认的构造器，就会抛出一个异常。<br />
<br />将forName与newInstance配合起来使用，可以根据存储在字符串中的类名创建一个对象。

```java
String s = "java.util.Random";
Object m = Class.forName(s).newInstance();
```

**注释：如果需要以这种方式向希望按名称创建的类的构造器提供参数，就不要使用上面那条语句，而必须使用Constructor类中的newInstance方法。**

<a name="fPjTj"></a>
#### 5.7.2 捕获异常

我们将在第7章中全面地讲述异常处理机制，但现在时常遇到一些方法需要抛出异常。<br />
<br />当程序运行过程中发生错误时，就会“抛出异常”。抛出异常比终止程序要灵活得很多，这是因为可以提供一个“捕获”异常的处理器（handler）对异常情况进行处理。<br />
<br />如果没有提供处理器，程序就会终止，并在控制台上打印出一条信息，其中给出了异常的类型。可能在前面已经看到过一些异常报告，例如，偶然使用了null引用或者数组越界等。<br />
<br />异常有两种类型：未检查异常和已检查异常。对于已检查异常，编译器将会检查是否提供了处理器。然而，有很多常见的异常，例如，访问null引用，都属于未检查异常。编译器不会查看是否为这些错误提供了处理器。毕竟，应该精心地编写代码来避免这些错误的发生，而不要将精力花在编写异常处理器上。<br />
<br />并不是所有的错误都是可以避免的。如果竭尽全力还是发生了异常，编译器就要求提供一个处理器。Class.forName方法就是一个抛出已检查异常的例子。在第7章中，将会看到几种异常处理的策略。现在，只介绍一下如何实现最简单的处理器。<br />
<br />将可能抛出已检查异常的一个或多个方法调用代码放在try块中，然后在catch子句中提供处理器代码。

```java
try{
	statements that might throw exceptions
}cath(Exception e{
    handler action
}
```
下面是一个示例：

```java
try{
	String name =...;//get class name
    Class cl = Class.forName(name);//might throw exception
   	do something with cl
}catch(Exception e){
	e.printStackTrace();
}
```

<br />如果类名不存在，则将跳过try块中的剩余代码，程序直接进入catch子句（这里，利用Throwable类的printStackTrace方法打印出栈的轨迹。Throwable是Exception类的超类）。如果try块中没有抛出任何异常，那么会跳过catch子句的处理器代码。<br />
<br />对于已检查异常，只需要提供一个异常处理器。可以很容易地发现会抛出已检查异常的方法。如果调用了一个抛出已检查异常的方法，而又没有提供处理器，编译器就会给出错误报告。<br />
<br />

<a name="DyKtj"></a>
#### 5.7.3 利用反射分析类的能力
下面简要地介绍一下反射机制最重要的内容——检查类的结构。<br />
<br />在java.lang.reflect 包中有三个类Field、Method和Constructor分别用于描述类的域、方法和构造器。这三个类都有一个叫做getName的方法，用来返回项目的名称。Field类有一个getType方法，用来返回描述域所属类型的Class对象。Method和Constructor类有能够报告参数类型的方法，Method类还有一个可以报告返回类型的方法。这三个类还有一个叫做getModifiers的方法，它将返回一个整型数值，用不同的位开关描述public 和 static 这样的修饰符使用状况。另外，还可以利用java.lang.reflect包中的Modifier类的静态方法分析getModifiers返回的整型数值。例如，可以使用Modifier类中的isPublic、isPrivate或者isFinal判断方法或构造器是否是public 、private或final。我们需要做的全部工作就是调用Modifier类的相应方法，并对返回的整型数值进行分析，另外，还可以利用Modifier.toString方法将修饰符打印出来。<br />
<br />Class类中的getFields、getMethods和getConstructors方法将分别返回类提供的public域、方法和构造器数组，其中包括超类的公有成员。Class类的getDeclareFields、getDeclareMethods和getDeclaredConstructors方法将分别返回类中声明的全部域、方法和构造器、其中包括私有和受保护成员，但不包括超类的成员。<br />
<br />


<a name="42kD0"></a>
#### 5.7.4 在运行时使用反射分析对象
<a name="VA6Nh"></a>
#### 5.7.5 使用反射编写泛型数组代码
<a name="Z31Mn"></a>
#### 5.7.6 调用任意方法

<a name="ojmUL"></a>
### 5.8 继承的设计技巧

<a name="0cg4R"></a>
## 第6章 接口、lambda表达式与内部类
<a name="D7kdG"></a>
### 6.1 接口
<a name="2bAYd"></a>
### 6.2 接口示例


<a name="JM1Ry"></a>
### 6.3 lambda表达式

<a name="FjmT4"></a>
#### 6.3.1 为什么引入lambda表达式

<br />lambda 表达式是一个可传递的代码块，可以在以后执行一次或多次。

<a name="P0Wja"></a>
#### 6.3.2 lambda表达式的语法


<a name="AjBrb"></a>
#### 6.3.3 函数式接口
<a name="HVcpo"></a>
#### 6.3.4 方法引用
<a name="TpMy7"></a>
#### 6.3.5 构造器引用
<a name="9ThLH"></a>
#### 6.3.6 变量作用域
<a name="3Tiiq"></a>
#### 6.3.7 处理lambda表达式
<a name="Up9DV"></a>
#### 6.3.8 再谈Comparator

<a name="oH1wy"></a>
### 6.4 内部类

内部类（inner class）是定义在另一个类中的类。为什么需要使用内部类呢？其主要原因以下三点：

- **内部类方法可以访问该类定义所在的作用域中的数据，摆阔私有的数据。**
- **内部类可以对同一个包中的其他类隐藏起来。**
- **当想要定义一个回调函数且不想编写大量代码时，使用匿名（anonymous）内部类比较便捷。**

<a name="u0ixs"></a>
#### 6.4.1 使用内部类访问对象状态

内部类的语法比较复杂。鉴于此情况，我们选择一个简单但不太实用的例子说明颞部类的使用方式。下面将进一步分析 **TimerTest **示例，并抽象出一个 **TalkingClock **类。构造一个语音时钟需要提供两个参数：发布通告的间隔和开关铃声的标志。

```java
public class TalkingClock{
	private int interval;
    private boolean beep;
    
    public TalkingClock(int interval, boolean beep){...}
    public void start(){...}
    public class TimePrinter implements ActionListener{...}
}
```

需要注意，这里的 **TimePrinter **类位于 **TalkingClock **类内部。这并不意味着每个 **TalkingClock **都有一个 **TimePrinter **实例域。如前所示， **TimePrinter **对象是由 **TalkingClock **类的方法构造。<br />
<br />下面是 TimePrinter 类的详细内容。需要注意一点，actoinPerformed 方法在发出铃声之前检查了 beep 标志。<br />

```java
public class TimePrinter implements ActionLitener{
	public void actionPerformed(ActionEvent event){
    	System.out.println("At the tone, the time is" + new Date()):
        if(beep){
        	Toolkit.getDefaultToolKit().beep();
        }
    }
}
```

令人惊讶的事情发生了。**TimePrinter **类没有实例域或者名为 **beep** 的变量，取而代之的是 **beep** 引用了创建 **TimePrinter **的域。这是一种创新的想法。从传统意义上讲，一个方法可以引用调用这个方法的对象数据域。内部类既可以访问自身的数据域，也可以访问创建它的外围类对象的数据域。<br />
<br />为了能够运行这个程序，内部类的对象总有一个隐式引用，它指向了创建它的外部类对象。如图6-3所示。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1573020294676-c7c31210-7e83-40f4-b0bd-c55602a6d79d.png#align=left&display=inline&height=248&name=image.png&originHeight=473&originWidth=709&search=&size=208070&status=done&width=371)

这个引用在内部类的定义中是不可见的。然而，为了说明这个概念，我们将外围类对象的引用称为outer。于是 actionPerformed 方法将等价于下列形式：

```java
public void actionPerformed(ActionEvent event){
	System.out.println("At the tone, the time is " + new Date());
    if(outer.beep){
    	Toolkit.getDefaultToolkit().beep();
    }
}
```

外围类的引用在构造器中设置。编译器修改了所有的内部类的构造器，添加一个外围类引用的参数。因为 TimePrinter 类没有定义构造器，所以编译器为这个类生成了一个默认的构造器，其代码如下所示：

```java
public TimePrinter(TalkingClock clock){
	outer = clock;
}
```
请再注意一下，outer 不是 Java 的关键字。我们只是用它说明内部类中的机制。<br />当在 start 方法中创建了 TimePrinter 对象后，编译器就会将 this 引用传递给当前的语音时钟的构造器：

```java
ActionListner listener = new TimePrinter(this);//Parameter automatically added
```

程序清单6-7给出了一个测试内部类的完整程序。下面我们再看一下访问控制。如果有一个 TimePrinter 类是一个常规类，它就需要通过 TalkingClock 类的公有方法访问 beep 标志，而使用内部类可以给予改进，即不必提供仅用于访问其他类的访问器。<br />
<br />**注释：TimePrinter 类声明为私有的。这样一来，只有 TalkingClock 的方法才能够构造 TimePrinter 对象。只有内部类可以是私有类，而常规类只可以具有包可见性，或公有可见性。**



<a name="iUkP8"></a>
#### 6.4.2 内部类的特殊语法规则
<a name="3xcSv"></a>
#### 6.4.3 内部类是否有用、必要和安全
<a name="4TU3i"></a>
#### 6.4.4 局部内部类
<a name="TM8LL"></a>
#### 6.4.5 由外部方法访问变量
<a name="qoNnv"></a>
#### 6.4.6 匿名内部类
<a name="vfKxO"></a>
#### 6.4.7 静态内部类

<a name="RC966"></a>
### 6.5 代理

<a name="g5HFk"></a>
## 第7章 异常、断言和日志
<a name="TRCwq"></a>
### 7.1 处理错误

<a name="PF55p"></a>
#### 7.1.1 异常分类
在Java程序设计语言中，异常对象都是派生于 Throwable 类的一个实例。稍后还可以看到，如果 Java 中内置的异常类不能够满足需求，用户可以创建自己的异常类。<br />图7-1是Java异常层次结构的一个简化示意图。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572686153449-2c83ab1d-ff77-4d64-b8c7-d881781d88de.png#align=left&display=inline&height=288&name=image.png&originHeight=575&originWidth=947&search=&size=243951&status=done&width=473.5)<br />
<br />需要注意的是，所有的异常都是由 Throwable 继承而来，但在下一层立即分解为两个分支：Error 和 Exception。<br />Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误。应用程序不应该抛出这种类型的对象。如果出现了这样的内部错误，除了通告给用户，并尽力使程序安全地终止之外，再也无能为力了。这种情况很少出现。<br />
<br />在设计Java程序是，需要关注Exception层次结构。这个层次结构又分解为两个分支：一个分支派生于 **RuntieException** ；另一个分支包含其他异常。划分两个分支的规则是：由程序错误导致的异常属于 **RuntimeException **；而程序本身没有问题，但由于想 I/O 错误这类问题导致的异常属于其他异常。<br />
<br />派生于RuntimeException的异常包含下面几种情况：

- 错误的类型转换。
- 数组访问越界。
- 访问null指针。


<br />不是派生与 RuntimeException的异常包括：

- 试图在文件尾部后面读取数据。
- 试图打开一个不存在的文件。
- 试图根据给定的字符串查找Class对象，而这个字符串表示的类并不存在。


<br />“如果出现 **RuntimeException**异常，那么就一定是你的问题”是一条相当有道理的规则。应该通过检测数组下标是否越界来避免 **ArrayIndexOutOfBoundsException **异常；应该通过使用变量之前检测是否为 null 来杜绝 **NullPointerException **异常的发生。<br />
<br />如何处理不存在的文件呢？难道不能先检查文件是否存在再打开它吗？嗯，这个文件有可能在你检查它是否存在之前就已经被删除了。因此，“是否存在”取决于环境，而不只是取决于你的代码。<br />
<br />Java 语言规范将派生于 Error 类或 RuntimeException 类的所有异常称为非受查（unchecked）异常，所有其他的异常称为受查（checked）异常。这是两个很有用的术语，在后面还会用到。编译器将核查是否为所有的受查异常提供了异常处理器。<br />
<br />

<a name="A0yw8"></a>
#### 7.1.2 声明受查异常
如果遇到了无法处理的情况，那么Java 的方法可以抛出一个异常。这个道理很简单：一个方法不仅需要告诉编译器将要返回什么值，还要告诉编译器有可能发生什么错误。例如，一段读取文件的代码知道有可能读取的文件不存在，或者内容为空，因此，试图处理文件信息的代码就需要通知编译器可能会抛出 **IOException **类的异常。<br />
<br />方法应该在其首部声明所有可能抛出的异常。这样可以从首部反映出这个方法可能抛出哪类受查异常。例如，下面是标准类库中提供的 FileInputStream 类的一个构造器的声明（有关输入和输出的更多信息请参看卷2 的第2章。）


<br />

<a name="aPJBR"></a>
#### 7.1.3 如何抛出异常
<a name="WTCfr"></a>
#### 7.1.4 创建异常类

<a name="P3vyY"></a>
### 7.2 捕获异常
<a name="fFYtO"></a>
### 7.3 使用异常机制的技巧
<a name="wOAxF"></a>
### 7.4 使用断言
<a name="UTFjb"></a>
### 7.5 记录日志
<a name="fommz"></a>
### 7.6 调试技巧


<a name="j9wtF"></a>
## 第8章 泛型程序设计
<a name="OG1Jc"></a>
## 第9章 集合

<a name="AXXvc"></a>
### 9.1 Java集合框架
<a name="emZBr"></a>
#### 9.1.1 将集合的接口与实现分离

<br />与现代的数据结构类库的常见情况一样，Java集合类库也将接口（interface）与实现（imlementation）分离。首先，看一下人们熟悉的数据结构——队列（queue）是如何分离的。<br />队列接口指出可以在队列的尾部添加元素，在队列的头部删除元素，并且可以查找队列中元素的个数。当需要收集对象，并按照“先进先出”的规则检索对象时就应该使用队列。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572701445745-347b5ebb-5ce6-440e-85b2-4a1ba6463140.png#align=left&display=inline&height=264&name=image.png&originHeight=392&originWidth=795&search=&size=171540&status=done&width=536)<br />


队列接口的最简形式可能类似下面这样：

```java
public interface Queue<E>{
	void add(E element);
    E remove();
    int size();
}
```

这个接口并没有说明队列是如何实现的。队列通常有两种实现方式：一种是使用循环数组；另一种是使用链表。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/214061/1572701631065-ff65f34f-b539-4c02-8fb2-fc2cd482d2e4.png#align=left&display=inline&height=319&name=image.png&originHeight=638&originWidth=900&search=&size=373619&status=done&width=450)

<a name="W72wP"></a>
#### 9.1.2 Collection接口

在Java类库中，集合类的基本接口是Collection接口。这个接口有两个基本方法：

```java
public interface Collection<E>{
	boolean add(E element);
    Iterator<E> iterator();
    ...
}
```

除了这两个方法之外，还有几个方法，将在稍后介绍。<br />add方法用于向集合中添加元素。如果添加元素确实改变了集合就返回true，如果集合没有发生变化就返回false。例如，如果试图向集中添加一个对象，而这个对象在集中已经存在，这个添加请求就没有实效，因为集中不允许有重复的对象。<br />**iterator**方法用于返回一个实现了**Iterator**接口的对象。可以使用这个迭代器对象依次访问集合中的元素。


<a name="4tt21"></a>
#### 9.1.3 迭代器

Iterator接口包含4个方法：

```java
public interface Iterator<E>{
	E next();
    bolean hasNext();
    void remove();
    default void forEachRemaining(Consumer<? super E> action);
}
```

<br />通过反复调用next方法，可以逐个访问集合中的每个元素。但是，如果到达了集合的末尾，next方法将抛出一个NoSuchElementException。因此，需要在调用next之前调用hasNext方法。如果迭代器对象还有多个供访问的元素，这个方法就返回true。如果想要查看集合中的所有元素，就请求一个迭代器，并在hasNext返回true时反复地调用next方法。例如：

```java
Collection<String> c= ...;
Iterator<String> iter = c.iterator();
while(iter.hasNext()){
	String element = iter.next();
    do something with element
}
```

<br />用“for each”循环可以更加简练地表示同样的循环操作：

```java
for(String element:c){
	do something with element
}
```

<br />编译器简单地将“for each”循环翻译为带有迭代器的循环。<br />“for each”循环可以与任何实现了Iterable接口的对象一起工作，这个接口只包含一个抽象方法：

```java
public interface Iterable<E>{
	Iterator<E> iterator();
    ...
}
```

<br />Collection 接口扩展了Iterable接口。因此，对于标准类库中的任何集合都可以使用“foreach”循环。<br />
<br />在Java SE 8 中，甚至不用写循环。可以调用 forEachRemaining 方法并提供一个 lambda 表达式（它会处理一个元素）。将对迭代器的每一个元素调用这个 lambda 表达式，直到再没有元素为止。<br />

```java
iterator.forEachRemaining(element -> dosomething& with element);
```

<br />
<br />
<br />

<a name="tWOi9"></a>
#### 9.1.4 泛型实用方法
<a name="nu7eV"></a>
#### 9.1.5 集合框架中的接口

<a name="rymIV"></a>
### 9.2 具体的集合

<a name="QEg9W"></a>
#### 9.2.1 链表
<a name="wxGSj"></a>
#### 9.2.2 数组列表
<a name="uOFvJ"></a>
#### 9.2.3 散列集
<a name="mTXNN"></a>
#### 9.2.4 树集
<a name="Yce23"></a>
#### 9.2.5 队列与双端队列
<a name="IhOds"></a>
#### 9.2.6 优先级队列

<a name="TAf1z"></a>
### 9.3 映射

<a name="Pt9D2"></a>
#### 9.3.1 基本映射操作
<a name="TfoVK"></a>
#### 9.3.2 更新映射项
<a name="Iw68t"></a>
#### 9.3.3 映射视图
<a name="g3xMR"></a>
#### 9.3.4 弱散列映射
<a name="OqUXR"></a>
#### 9.3.5 链接散列集与映射
<a name="27APg"></a>
#### 9.3.6 枚举集与映射
<a name="8gGQW"></a>
#### 9.3.7 标识散列映射

<a name="LEzwI"></a>
### 9.4 视图与包装器

<a name="9lFUb"></a>
#### 9.4.1 轻量级集合包装器
<a name="HQiht"></a>
#### 9.4.2 子范围
<a name="xvIiJ"></a>
#### 9.4.3 不可修改的视图
<a name="PpObx"></a>
#### 9.4.4 同步视图
<a name="qVawA"></a>
#### 9.4.5 受查视图
<a name="AETj7"></a>
#### 9.4.6 关于可选操作的说明

<a name="3n87A"></a>
### 9.5 算法

<a name="7vzIW"></a>
#### 9.5.1 排序与混排
<a name="nSEEX"></a>
#### 9.5.2 二分查找
<a name="jLqhW"></a>
#### 9.5.3 简单算法
<a name="ZUfcT"></a>
#### 9.5.4 批操作
<a name="IuYMw"></a>
#### 9.5.5 集合与数组的转换
<a name="q15aH"></a>
#### 9.5.6 编写自己的算法

<a name="3FZuC"></a>
### 9.6 遗留的集合

<a name="OT2L0"></a>
#### 9.6.1 Hashtable类
<a name="s7ffN"></a>
#### 9.6.2 枚举
<a name="96ZVX"></a>
#### 9.6.3 属性映射
<a name="d8YTi"></a>
#### 9.6.4 栈
<a name="a9tnI"></a>
#### 9.6.5 位集

<a name="Wm2Qy"></a>
## 第10章 图形程序设计
<a name="Pp9QT"></a>
## 第11章 事件处理
<a name="SZcpm"></a>
## 第12章 Swing用户界面组件
<a name="QwQ4i"></a>
## 第13章 部署Java应用程序
<a name="9HREh"></a>
## 第14章 并发

---

<a name="fegik"></a>
## 推荐博客


---

<a name="7LGAp"></a>
## 邀请

<br />感谢各位观看我的文章，为了和大家一起交流，以及分享个人心得，我建立了一个微信公众号，“零创世界”，我会每天在上面分享自己的学习心得，或者各位认为投稿，我会把各位的投稿放到微信微信公众号，让大家可以共同学习，共同进步。我还会在公众号上，发布一些如破解IDEA方法的文章及资源。在此谢谢！！！<br />
<br />以下就是一些联系方式：<br />

- **微信公众号**

![qrcode_for_gh_b9e5fa1667b4_344.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/214061/1571149395826-98f998eb-94c4-41fb-98bd-04185a4782e9.jpeg#align=left&display=inline&height=344&name=qrcode_for_gh_b9e5fa1667b4_344.jpg&originHeight=344&originWidth=344&search=&size=8339&status=done&width=344)

- **个人邮箱：17875512004@163.com**



