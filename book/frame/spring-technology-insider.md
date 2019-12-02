# 熟读《Spring技术内幕：深入解析Spring架构与设计原理》


---

## 第1章 Spring的设计理念和整体架构


## 第2章 Spring Framework的核心：IoC容器的实现

**本章内容**
* Spring IoC容器概述
* IoC容器系列的设计与实现：BeanFactory和AppliCationConteXt
* IC容器的初始化过程
* IoC容器的依赖注入
* 容器其他相关特性的设计与实现

### 2.1 Spring IoC容器概述
#### 2.1.1 IoC容器和依赖反转模式

&emsp;子日:温故而知新。在这里,我们先简要地回顾一下依赖反转的相关概念。我们选取维基百科中关于依赖反转的叙述,把这些文字作为我们理解依赖反转这个概念的参考。这里不会对这些原理进行学理上的考究,只是希望提供一些有用的信息,以便给读者一些启示。这个模式非常重要,它是IoC容器得到广泛应用的基础。

><center>维基百科对“依赖反转”相关概念的叙述</center>
>早在2004年, Martin Fowler就提出了“哪些方面的拉制被反转了?”这个问题。他得出的结论是:依赖对象的获得被反转了。基于这个结论,他为控制反转创造了一个史好的名字:依赖注入。许多非凡的应用(比 Helloworld.java史加优美、史加复杂)都是由两个或多个类通过彼此的合作来实现业务逻辑的,这使得每个对象都需要与其合作的对象(也就是它所依的对象)的引用。如果这个获取过程要靠自身实现,那么如你所见,这将导致代码高度耦合并且难以测试。

&emsp;以上的这段话概括了依赖反转的要义,如果合作对象的引用或依赖关系的管理由具体对象来完成,会导致代码的高度耦合和可测试性的降低,这对复杂的面向对象系统的设计是非常不利的。在面向对象系统中,对象封装了数据和对数据的处理,对象的依赖关系常常体现在对数据和方法的依赖上。这些依赖关系可以通过把对象的依赖注入交给框架或IoC容器来完成,这种从具体对象手中交出控制的做法是非常有价值的,它可以在解耦代码的同时提高代码的可测试性。在极限编程中对单元测试和重构等实践的强调体现了在软件开发过程中对质量的承诺,这是软件项目成功的一个重要因素。

&emsp;依赖控制反转的实现有很多种方式。在 Spring中,IoC容器是实现这个模式的载体,它可以在对象生成或初始化时直接将数据注入到对象中,也可以通过将对象引用注入到对象数据域中的方式来注入对方法调用的依赖。这种依赖注入是可以递归的,对象被逐层注入。就此而言,这种方案有一种完整而简洁的美感,它把对象的依赖关系有序地建立起来,筒化了对象依赖关系的管理,在很大程度上简化了面向对象系统的复杂性。

&emsp;关于如何反转对依赖的控制,把控制权从具体业务对象手中转交到平台或者框架中,是降低面向对象系统设计复杂性和提高面向对象系统可测试性的一个有效的解决方案。它促进了IoC设计模式的发展,是IoC容器要解决的核心问题。同时,也是产品化的IoC容器出现的推动力。

> **注意：** IoC亦称为“依赖倒置原理”( Dependency Inversion Principle),几乎所有框架都使用了倒置注入( Martin Fowler)技巧,是IoC原理的一项应用。 Smalltalk、C++、Java和.NET等画向对象语言的程序员已使用了这些原理。控制反转是 Spring框架的核心。

&emsp;IoC原理的应用在不同的语言中有很多实现,比如 Smalltalk、C++、Java等。在同一语言的实现中也会有多个具体的产品, Spring是Java语言实现中最著名的一个。同时,IoC地是Springi框架要解决的核心问题。

>**注意：**应用控制反转后,当对象被创建时,由一个调控系统内的所有对象的外界实体将其所依赖的对象的引用传递给它,即依赖被注入到对象中。所以,控制反转是关于一个对象如何获取它所位赖的对象的引用,在这里,反转指的是责任的反转。

&emsp;我们可以认为上面提到的调控系统是应用平台,或者更具体地说是IoC容器。通过使用IoC容器,对象依赖关系的管理被反转了,转到IoC容器中来了,对象之间的相互依赖关系由loC容器进行管理,并由Ioc容器完成对象的注入。这样就在很大程度上简化了应用的开发,把应用从复杂的对象依赖关系管理中解放出来。简单地说,因为很多对象依赖关系的建立和维护并不需要和系统运行状态有很强的关联性,所以可以把在面向对象编程中需要执行的诸如新建对象、为对象引用赋值等操作交由容器统一完成。这样一来,这些散落在不同代码中的功能相同的部分就集中成为容器的一部分,也就是成为面向对象系统的基础设施的一部分。


&emsp;如果对面向对象系统中的对象进行简单分类,会发现除了一部分是数据对象外,其他很大一部分对象是用来处理数据的。这些对象并不常发生变化,是系统中基础的部分。在很多情况下,这些对象在系统中以单件的形式起作用就可以满足应用的需求,而且它们也不常涉及数据和状态共享的问题。如果涉及数据共享方面的问题,需要在这些单件的基础上再做进步的处理。

&emsp;同时,这些对象之间的相互依赖关系也是比较稳定的,一般不会随着应用的运行状态的改变而改变。这些特性使这些对象非常适合由IoC容器来管理,虽然它们存在于应用系统中,但是应用系统并不承担管理这些对象的责任,而是通过依赖反转把责任交给了容器(或者说平台)。了解了这些背景, Spring loc容器的原理也就不难理解了。在原理的具体实现上,Spring有着自己的独特思路、实现技巧和丰富的产品特性。关于这些原理的实现,下面会进行详细的分析。




#### 2.1.2 Spring IoC的应用场景

&emsp;在 Java EE'企业应用开发中,前面介绍的IoC(控制反转)设计模式,是解耦组件之间复杂关系的利器, Spring Ioc模块就是这个模式的一种实现。在以 Spring为代表的轻量级 Java EE-开发风行之前,我们更熟悉的是以EIB为代表的开发模式。在EJB模式中,应用开发人员需要编写EJB组件,而这种组件需要满足EJB容器的规范,才能够运行在EJB容器中,从而获取事务管理、生命周期管理这些组件开发的基本服务。从获取的基本服务上看, Spring1提供的服务和EJB容器提供的服务并没有太大的差别,只是在具体怎样获取服务的方式上,两者的设计有很大的不同:在 Spring中, Spring loc提供了一个基本的 Javabean?容器,通过IoC模式管理依赖关系,并通过依赖注入和AOP切面增强了为 Java Bean这样的POJO对象赋予事务管理、生命周期管理等基本功能;而对于EJB,一个简单的EJB组件需要编写远程/本地接口、Home接口以及Bean的实现类,而且EJB运行是不能脱离EIB容器的,查找其他EB组件也需要通过诸如JND这样的方式,从而造成了对EJB容器和技术规范的依赖。也就是说 Springi把EJB组件还原成了POJO对象或者 Javabean对象,降低了应用开发对传统J2EE技术规范的依赖。


&emsp;同时,在应用开发中,以应用开发人员的身份设计组件时,往往需要引用和调用其他组件的服务,这种依赖关系如果固化在组件设计中,就会造成依赖关系的僵化和维护难度的增加,这个时候,如果使用IoC容器,把资源获取的方向反转,让IoC容器主动管理这些依赖关系,将这些依赖关系注入到组件中,那么会让这些依赖关系的适配和管理更加灵活。在具体的注入实现中,接口注入(Type1IoC)、 setter注入(Type2IoC)、构造器注入(Type3IoC)是主要的注入方式。在 Spring的IoC设计中,seer注入和构造器注入是主要的注入方式;相对而言,使用 Spring时 setter?注人是常见的注入方式,而且为了防止注入异常, Spring loc容器还提供了对特定依赖的检査。

&emsp;另一方面,在应用管理依赖关系时,可以通过IoC容器将控制进行反转,在反转的实现中,如果能通过可读的文本来完成配置,并且还能通过工具对这些配置信息进行可视化的管理和浏览,那么肯定是能够提高对组件关系的管理水平,并且如果耦合关系需要变动,并不需要重新修改和编译Java源代码,这符合在面向对象设计中的开闭准则,并且能够提高组件系统设计的灵活性,同时,如果结合OSGi的使用特性,还可以提高应用的动态部署能力。

&emsp;在具体使用 Spring loc容器的时候,我们可以看到, Spring loc容器已经是一个产品实现。作为产品实现,它对多种应用场景的适配是通过 Spring设计的IoC容器系列来实现的,比如在某个容器系列中可以看到各种带有不同容器特性的实现,可以读取不同配置信息的各种容器,从不同I/O源读取配置信息的各种容器设计,更加面向框架的容器应用上下文的容器设计等。这些丰富的容器设计,已经可以满足广大用户对IoC容器的各种使用需求,这时的Spring Ioc容器,已经不是原来简单的 Interface21框架了,已经成为一个1oC容器的工业级实现。下面,我们会对IoC容器系列的设计和实现进行详细分析。

### 2.2 IoC容器系列的设计与实现：BeanFactory和AppliCationConteXt

&emsp;在 Spring Ioc容器的设计中,我们可以看到两个主要的容器系列,一个是实现Beanfactory接口的简单容器系列,这系列容器只实现了容器的最基本功能;另一个是Application Context应用上下文,它作为容器的高级形态而存在。应用上下文在简单容器的基础上,增加了许多面向框架的特性,同时对应用环境作了许多适配。有了这两种基本的容器系列,基本上可以满足用户对IoC容器使用的大部分需求了。下面,我们就对 Spring Ioc容器中这两种容器系列的设计与实现进行一个简要的分析。

#### 2.2.1 Spring的IoC容器系列

&emsp;IoC容器为开发者管理对象之间的依赖关系提供了很多便利和基础服务。有许多IoC容器供开发者选择, Springframework的IoC核心就是其中ー个,它是开源的。那具体什么是IoC容器呢?它在 Spring框架中到底长什么样?其实对1oC容器的使用者来说,我们经常接触到的Beanfactory和 Application Context都可以看成是容器的具体表现形式。如果深入到 Spring的实现中去看,我们通常所说的IoC容器,实际上代表着一系列功能各异的容器产品,只是容器的功能有大有小,有各自的特点。我们以水桶为例,在商店中出售的水桶有大有小,制作材料也各不相同,有金属的、塑料的等,总之是各式各样的,但只要能装水,具备水桶的基本特性,那就可以作为水桶来出售,来让用户使用。这在 Springi中也是一样, Spring有各式各样的IoC容器的实现供用户选择和使用。使用什么样的容器完全取决于用户的需要,但在使用之前如果能够了解容器的基本情况,那对容器的使用是非常有帮助的,就像我们在购买商品前对商品进行考察和挑选那样。从代码的角度入手,我可以看到关于这一系列容器的设计情况。

&emsp;就像商品需要有产品规格说明一样,同样,作为IoC容器,也需要为它的具体实现指定基本的功能规范,这个功能规范的设计表现为接口类 Bean Factory,它体现了 Spring为提供给用户使用的IoC容器所设定的最基本的功能规范。还是以前面的百货商店出售水桶为例,如果把IoC容器看成一个水桶,那么这个 Bean Factory就定义了可以作为水桶的基本功能,比如至少能装水,有个提手等。除了满足基本的功能,为了不同场合的需要,水桶的生产厂家还在这个基础上为用户设计了其他各式各样的水桶产品,以满足不同的用户需求。这些水桶会提供更丰富的功能,有简约型的,有豪华型的,等等。但是,不管是什么水桶,它都需要有项最基本的功能:能够装水。那对 Spring的具体IoC容器实现来说,它需要满足的基本特性是什么呢?它需要满足 Bean Factory这个基本的接口定义,所以在图2-1中可以看到这个Bean Factory接口在继承体系中的地位,它是作为一个最基本的接口类出现在 Spring的IoC容器体系中的。

&emsp;在这些 Spring提供的基本IoC容器的接口定义和实现的基础上, Spring通过定义Beandefinition来管理基于 Spring的应用中的各种对象以及它们之间的相互依赖关系。Bean Definition抽象了我们对Bean的定义,是让容器起作用的主要数据类型。我们都知道,在计算机世界里,所有的功能都是建立在通过数据对现实进行抽象的基础上的。IoC容器是用来管理对象依赖关系的,对1oC容器来说, Beandefinition就是对依赖反转模式中管理的对象依赖关系的数据抽象,也是容器实现依赖反转功能的核心数据结构,依赖反转功能都是围绕对这个 Beandefinitionl的处理来完成的。这些 Bean Definition就像是容器里装的水,有了这些基本数据,容器才能够发挥作用。在下面的分析中, Beandefinition的出现次数会很多。

&emsp;同时,在使用IoC容器时,了解 Bean Factory和 Application Context之间的区别对我们理解和使用IoC容器也是比较重要的。弄清楚这两种重要容器之间的区別和联系,意味着我们具备了辨别容器系列中不同容器产品的能力。还有一个好处就是,如果需要定制特定功能的容器实现,也能比较方便地在容器系列中找到一款恰当的产品作为参考,不需要重新设计。

#### 2.2.2 Spring IoC容器的设计

&emsp;在前面的小节中,我们了解了IoC容器系列的概况。在 Spring中,这个1oC容器是怎样设计的呢?我们可以看一下如图2-2所示的IoC容器的接口设计图,这张图描述了IoC容器中的主要接口设计。

![IOC容器d的接口设计图](http://q1q6z0e19.bkt.clouddn.com/1575191643503-e1050288-23cf-4dc4-a7a6-9896d270309e.png)


&emsp;下面对接口关系做一些简要的分析,可以依据以下内容来理解这张接口设计图。

* 从接口 Bean Factory到 Hierarchicalbean Factory,再到 Configurable Bean Factory,是条主要的 Bean Factory设计路径。在这条接口设计路径中, Bean Factory接口定义了基本的IoC容器的规范。在这个接口定义中,包括了 getbeano这样的IoC容器的基本方法(通过这个方法可以从容器中取得Bean)。而 Hierarchicalbeanfactory接口在继承了Beanfactory!的基本接口之后,增加了 getparentbean Factory的接口功能,使Bean Factory>具备了双亲oC容器的管理功能。在接下来的 Configurablebean Factory接口中,主要定义了一些对 Bean Factory的配置功能,比如通过 setparentbeanfactoryo)设置双亲IoC容器,通过 addbean Postprocessor(配置Bean后置处理器,等等。通过这些接口设计的叠加,定义了 Beanfactory就是简单oC容器的基本功能。关于 Bean Factory简单IoC容器的设计,我们会在后面的内容中详细介绍。

* 第二条接口设计主线是,以 Application Context应用上下文接口为核心的接口设计,这里涉及的主要接口设计有,从 Beanfactory到 Listablebeanfactory,再到Applicationcontext,再到我们常用的 Webapplicationcontext或者Configurable Application Context接口。我们常用的应用上下文基本上都是Configurable Application Context或者 Webapplication Context的实现。在这个接口体系中, Listablebean Factory和 Hierarchicalbean Factory两个接口,连接 Bean Factory接口定义和 Application Conext应用上下文的接口定义。在 Listable Bean Factory接口中,细化了许多 Bean Factory的接口功能,比如定义了 getbean Definitionnameso接口方法;对于 Hierarchicalbeanfactory接口,我们在前文中已经提到过;对于Applicationcontext接口,它通过继承 Messagesource、 Resourceloader、Application Eventpublisher接口,在 Bean Factory简单IoC容器的基础上添加了许多对高级容器的特性的支持。

* 这里涉及的是主要接口关系,而具体的IoC容器都是在这个接口体系下实现的,比如Defaultlistable Bean Factory,这个基本IoC容器的实现就是实现了 ConfigurableBeanfactory,从而成为一个简单IoC容器的实现。像其他IoC容器,比如Xmlbean Factory,都是在 Defaultlistable Bean Factoryl的基础上做扩展,同样地,Application Context的实现也是如此。


* 这个接口系统是以 Bean Factory和 Application Context,为核心的。而 Bean Factory又是loC容器的最基本接口,在 Application Contexta的设计中,一方面,可以看到它继承了Beanfactory接口体系中的 Listable Factory、 Autowire Capable Bean Factory、Hierarchical Bean Factory等 Beanfactoryl的接口,具备了 Beanfactory loc容器的基本功能;另一方面,通过继承 Messagesource、 Resourceloadr、 Applicationeventpublisher这些接口, Bean Factory为 Application Context赋予了更高级的loC容器特性。对于Application Context而言,为了在Web环境中使用它,还设计了 Webapplication Context接口,而这个接口通过继承 Themesource接口来扩充功能。


&emsp;**1. BeanFactory的应用场景**

&emsp;Bean Factory提供的是最基本的IoC容器的功能,关于这些功能定义,我们可以在接口Bean Factory中看到。

&emsp;Bean Factory接口定义了IoC容器最基本的形式,并且提供了IoC容器所应该遵守的最基本的服务契约,同时,这也是我们使用IoC容器所应遵守的最底层和最基本的编程规范,这些接口定义勾画出了1oC的基本轮廓。很显然,在 Spring的代码实现中, Beanfactory只是个接口类,并没有给出容器的具体实现,而我们在图2-1中看到的各种具体类,比如Defaultlistablebean Factory、 Xmibeanfactory、 Application Context等都可以看成是容器附加了某种功能的具体实现,也就是容器体系中的具体容器产品。下面我们来看看 Beanfactory是怎样定义IoC容器的基本接口的。

&emsp;用户使用容器时,可以使用转义符“&”来得到 Factory Bean本身,用来区分通过容器来获取Factorybeanj产生的对象和获取 Factory Bean本身。举例来说,如果 myjndiobject,是一个 Factory Bean那么使用& myjndiobjecti得到的是 Factory Bean,而不是 myjndiobjecti这个 Factorybeanj产生出来的对象。关于具体的 Factory Beang的设计和实现模式,我们会在后面的章节中介绍。

>**注意：** 理解上面这段话需要很好地区分 Factory Bean和 Bean Factory这两个在 Spring中使用频率很高的类,它们在拼写上非常相似。一个是 Factory,也就是IoC容器或对象エ厂;一个是Bean。在 Spring中,所有的Bean都是由 Beanfactory(也就是IoC容器)来进行管理的。但对 Factory Bean而言,这个Bean不是简单的Bean,而是一个能产生或者修饰对象生成的エ厂Bcan,它的实现与设计模式中的エ厂模式和修饰器模式类似。


&emsp;Bean Factory接口设计了 getbean方法,这个方法是使用IoC容器API的主要方法,通过这个方法,可以取得I0C容器中管理的Bean,Bean的取得是通过指定名字来索引的。如果需要在获取Bean时对Bean的类型进行检査, Bean Factory接口定义了带有参数的 getbean方法,这个方法的使用与不带参数的 get Bean方法类似,不同的是增加了对Bean检素的类型的要求。

&emsp;用户可以通过 Bean Factory接口方法中的 getbean来使用Bean名字,从而在获取Bean时,如果需要获取的Bean是 prototype类型的,用户还可以为这个 prototype类型的Bean生成指定构造函数的对应参数。这使得在一定程度上可以控制生成 prototype类型的Bean。有了Beanfactory的定义,用户可以执行以下操作:

* 通过接口方法 containsbeani让用户能够判断容器是否含有指定名字的Bean。
* 通过接口方法 issingleton来査询指定名字的Bean是否是Singleton类型的 Bean。对于Singleton属性,用户可以在 Bean Definitiont中指定。
* 通过接口方法 is Prototype来査询指定名字的Bean是否是 prototype类型的。与 Singleton属性一样,这个属性也可以由用户在 Beandefinition中指定。
* 通过接口方法 is'typematch来査询指定了名字的Bean的 Classy类型是否是特定的Clas类型。这个 Class2类型可以由用户来指定。
* 通过接口方法 gettype来査询指定名字的Bean的 Classy类型。
* 通过接口方法 getaliases来查询指定了名字的Bean的所有别名,这些别名都是用户在Bean Definition中定义的。

&emsp;这些定义的接口方法勾画出了IoC容器的基本特性。因为这个 Bean Factory接口定义了IoC容器,所以下面给出它定义的全部内容供大家参考,如代码清单2-1所示。

<center>代码清单2-1 BeanFactory接口</center>

```Java
public interface BeanFactory {

	String FACTORY_BEAN_PREFIX = "&";
	Object getBean(String name) throws BeansException;
	<T> T getBean(String name, Class<T> requiredType) throws BeansException;
	<T> T getBean(Class<T> requiredType) throws BeansException;
	Object getBean(String name, Object... args) throws BeansException;
	<T> T getBean(Class<T> requiredType, Object... args) throws BeansException;
	boolean containsBean(String name);
	boolean isSingleton(String name) throws NoSuchBeanDefinitionException;
	boolean isPrototype(String name) throws NoSuchBeanDefinitionException;
	boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;
	boolean isTypeMatch(String name, Class<?> typeToMatch) throws NoSuchBeanDefinitionException;
	Class<?> getType(String name) throws NoSuchBeanDefinitionException;
	String[] getAliases(String name);

}
```

&emsp;可以看到,这里定义的只是一系列的接口方法,通过这一系列的 Bean Factory接口,可以使用不同的Bean的检索方法,很方便地从IoC容器中得到需要的Bean,从而忽略具体的loC容器的实现,从这个角度上看,这些检索方法代表的是最为基本的容器入口。

&emsp;**2. BeanFactory容器的设计原理**

&emsp;Bean Factory接口提供了使用IoC容器的规范。在这个基础上, Spring还提供了符合这个IoC容器接口的一系列容器的实现供开发人员使用。我们以 Xmlbeanfactoryl的实现为例来说明简单IoC容器的设计原理。如图2-3所示为 Kmlbean Factory设计的类继承关系。

![XmlBeanFactory设计的类继承关系](http://q1q6z0e19.bkt.clouddn.com/1575197077996-f57f63db-0287-4d14-bfd6-e6397ceb66ca.png)

&emsp;可以看到,作为一个简单IoC容器系列最底层实现的XmlBeanFactory,与我们在 Spring应用中用到的那些上下文相比,有一个非常明显的特点:它只提供最基本的10C容器的功能理解这一点有助于我们理解 Application Context与基本的 Beanfactory之间的区别和联系。我们可以认为直接的 Beanfactory实现是IoC容器的基本形式,而各种 Application Contexte的实现是IoC容器的高级表现形式。关于 Applicationcontext的分析,以及它与 Beanfactory相比的增强特性都会在下面进行详细的分析。

&emsp;让我们好好地看一下图2-3中的继承关系,从中可以清楚地看到类之间的联系,它们都是IoC容器系列的组成部分。在设计这个容器系列时,我们可以从继承体系的发展上看到IoC容器各项功能的实现过程。如果要扩展自己的容器产品,建议读者最好在继承体系中检査一下,看看 Spring是不是已经提供了现成的或相近的容器实现供我们参考。下面就从我们比较熟悉的 Xmlbeanfactoryg的实现入手进行分析,来看看一个基本的IoC容器是怎样实现的。

&emsp;仔细阅读 Kmlbeanfactory的源码,在一开始的注释里会看到对 Xmlbeanfactory.功能的简要说明,从代码的注释还可以看到,这是 Rod Johnson在2001年就写下的代码,可见这个类应该是 Spring的元老类了。 Xmlbeanfactory继承自 Defaultlistablebeanfactory这个类,后者非常重要,是我们经常要用到的一个IoC容器的实现,比如在设计应用上下文Application Context时就会用到它。我们会看到这个 Defaultlistable Beanfactory实际上包含了基本IoC容器所具有的重要功能,也是在很多地方都会用到的容器系列中的一个基本产品。

&emsp;在 Spring中,实际上是把 Default Listablebean?作为一个默认的功能完整的IoC容器来使用的。 Xmlbeanfactory在继承了 Defaultlistable Beanfactory容器的功能的同时,增加了新的功能,这些功能很容易从 Xmlbeanfactory的名字上猜到。它是一个与XML相关的Bean Factory,也就是说它是一个可以读取以XML文件方式定义的 Bean Definition的IoC容器。


&emsp;这些实现XML读取的功能是怎样实现的呢?对这些XML文件定义信息的处理并不是由Xml Be a n F actory直接完成的。在Xm1 Be n Factory中,初始化了一个Kmi Definitionreader对象,有了这个 Reader对象,那些以XML方式定义的Bean Definition就有了处理的地方。我们可以看到,对这些XML形式的信息的处理实际上是由这个 Xmibeandefinitionreader来完成的。

&emsp;构造 Xmlbean Factory这个loC容器时,需要指定 Bean Definitiong的信息来源,而这个信息来源需要封装成 Spring中的 Resource2类来给出。 Resource是 Spring用来封装I/O操作的类。比如,我们的 Beandefinition信息是以XML文件形式存在的,那么可以使用像“ Classpath- Resourceres= new Classpathresource(" beans.xml");"”这样具体的 Classpathresource来构造需要的Resource,然后将 Resource1作为构造参数传递给 Kmlbeanfactory.构造函数。这样,IoC容器就可以方便地定位到需要的 Beandefinition'信息来对Bean完成容器的初始化和依赖注入过程。

&emsp;Xmlbean Factory的功能是建立在 Defaultlistable Beanfactory这个基本容器的基础上的,并在这个基本容器的基础上实现了其他诸如XML读取的附加功能。对于这些功能的实现原理,看一看 Kmlbean Factory的代码实现就能很容易地理解。如代码清单2-2所示,在Xmlbeanfactory构造方法中需要得到 Resource)对象。对 Xmibeandefinitionreader对象的初始化,以及使用这个对象来完成对 load Bean Definitions的调用,就是这个调用启动从 Resource中载入 Bean Definitionsl的过程, Load Beandefinitions同时也是1oC容器初始化的重要组成部分。


代码清单2-2 XmlBeanFactory的实现
```Java
public class XmlBeanFactory extends DefaultListableBeanFactory {

	private final XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(this);


	public XmlBeanFactory(Resource resource) throws BeansException {
		this(resource, null);
	}

	public XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory) throws BeansException {
		super(parentBeanFactory);
		this.reader.loadBeanDefinitions(resource);
	}
}
```

&emsp;我们看到 Xmibean Factory使用了 Defaultlistable Beanfactory作为基类, DefaultlistableBeanfactory是很重要的一个IoC实现,在其他IoC容器中,比如 Application Context,其实现的基本原理和 Xmlbean Factory一样,也是通过持有或者扩展 Defaultlistablebean Factory来获得基本的IoC容器的功能的。

&emsp;参考 Xmlbeanfactory的实现,我们以编程的方式使用 Defaultlistablebeanfactory。从中我们可以看到IoC容器使用的一些基本过程。尽管我们在应用中使用IoC容器时很少会使用这样原始的方式,但是了解一下这个基本过程,对我们了解IoC容器的工作原理是非常有帮助的。因为这个编程式使用容器的过程,很清楚揭示了在IoC容器实现中的那些关键的类(比如 Resource、 Defaultlistable Beanfactory和 Beandefinitionreader)之间的相互关系,例如它们是如何把IoC容器的功能解耦的,又是如何结合在一起为IoC容器服务的,等等。在代码清单2-3中可以看到编程式使用IoC容器的过程。

**<center>代码清单2-3 编程式使用IOC容器</center>**

```Java
ClassPathResource res = new ClassPathResource("beans.xml");
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
reader.loadBeanDefinitions(res);
```

&emsp;这样,我们就可以通过 factory对象来使用 Defaultlistable Bean Factory这个IoC容器了。在使用IoC容器时,需要如下几个步骤:
1) 创建1oC配置文件的抽象资源,这个抽象资源包含了 Bean Definition的定义信息。
2) 创建一个 Beanfactory,这里使用 DefaultlistableBeanfactory。
3) 创建一个载入 Bean Definitionl的读取器,这里使用 Xmlbeandefinitionreader来载入XML文件形式的 BeanDefinition,通过一个回调配置给 Beanfactory。
4) 从定义好的资源位置读入配置信息,具体的解析过程由 Xmlbeandefinitionreader来完成。完成整个载入和注册Bean定义之后,需要的1oC容器就建立起来了。这个时候就可以直接使用IoC容器了。


&emsp;**3. ApplicationContext的应用场景**

&emsp;上一节中我们了解了IoC容器建立的基本步骤。理解这些步骤之后,可以很方便地通过编程的方式来手工控制这些配置和容器的建立过程了。但是,在 Spring中,系统已经为用户提供了许多已经定义好的容器实现,而不需要开发人员事必躬亲。相比那些简单拓展Beanfactoryp的基本IoC容器,开发人员常用的 Application:除了能够提供前面介绍的容器的基本功能外,还为用户提供了以下的附加服务,可以让客户更方便地使用。所以说,Application Context是一个高级形态意义的IoC容器,如图24所示,可以看到 Application Context在 Beanfactory的基础上添加的附加功能,这些功能为 Applicationcontext提供了以下Beanfactory不具备的新特性。

* 支持不同的信息源。我们看到 Application Context扩展了 Messagesourcef接口,这些信息源的扩展功能可以支持国际化的实现,为开发多语言版本的应用提供服务。
* 访问资源。这一特性体现在对 Resourceloaderi和 Resource的支持上,这样我们可以从不同地方得到Bean定义资源。这种抽象使用户程序可以灵活地定义Bean定义信息,尤其是从不同的I/O途径得到Bean定义信息。这在接口关系上看不出来,不过一般来说,具体 Application Context都是继承了 Defaultresourceloader的子类。因为Defaultresource Loader是 Abstract Application Context的基类,关于 Resource?在IoC容器中的使用,后面会有详细的讲解。
* 支持应用事件。继承了接口 Applicationeventpublisher,从而在上下文中引入了事件机制。这些事件和Bean的生命周期的结合为Bean的管理提供了便利。
* 在 Application Contexte中提供的附加服务。这些服务使得基本IoC容器的功能更丰富。因为具备了这些丰富的附加功能,使得 Application Context与简单的 Beanfactory相比,对它的使用是一种面向框架的使用风格,所以一般建议在开发应用时使用Application Context作为IoC容器的基本形式。


&emsp;**4. ApplicationContext容器的设计原理**

&emsp;在 Application Context容器中,我们以常用的 Filesystem Context的实现为例来说明 Application Context容器的设计原理。

&emsp;在 Filesystem Xmlapplication Context的设计中,我们看到 Application Context应用上下文的主要功能已经在 Filesystemxmlapplication Contextl的基类 Abstract Xmlapplication Context中实现了,在 Filesystemxmlapplication Context中,作为一个具体的应用上下文,只需要实现和它自身设计相关的两个功能。

&emsp;一个功能是,如果应用直接使用 Filesystemxml Application Context,对于实例化这个应用上下文的支持,同时启动1oC容器的 refresh过程。这在 Filesystem Application Context的代码实现中可以看到,代码如下:

```Java
public FileSystemXmlApplicationContext(String[] configLocations, boolean refresh, ApplicationContext parent)
			throws BeansException {

		super(parent);
		setConfigLocations(configLocations);
		if (refresh) {
			refresh();
		}
	}
```

&emsp;这个 refresh()过程会牽涉IoC容器启动的一系列复杂操作,同时,对于不同的容器实现,这些操作都是类似的,因此在基类中将它们封装好。所以,我们在 Filesystemxmli的设计中看到的只是一个简单的调用。关于这个 refresh在IoC容器启动时的具体实现,是后面要分析的主要内容,这里就不详细展开了。

&emsp;另一个功能是与 Filesystemxml Application Context设计具体相关的功能,这部分与怎样从文件系统中加载XML的Bean定义资源有关。

&emsp;通过这个过程,可以为在文件系统中读取以XML形式存在的 Beandefinition做准备,因为不同的应用上下文实现对应着不同的读取 Beandefinition的方式,在 FilesystemxmiApplicationContext中的实现代码如下:

```Java
protected Resource getResourceByPath(String path) {
		if (path != null && path.startsWith("/")) {
			path = path.substring(1);
		}
		return new FileSystemResource(path);
	}
```

&emsp;可以看到,调用这个方法,可以得到 Filesystemresourcef的资源定位。

### 2.3 IC容器的初始化过程

&emsp;简单来说,IoC容器的初始化是由前面介绍的 refresh()方法来启动的,这个方法标志着IoC容器的正式启动。具体来说,这个启动包括 Bean Definitiong的 Resouce'定位、载入和注册三个基本过程。如果我们了解如何编程式地使用IoC容器,就可以清楚地看到 Resource'定位和载入过程的接口调用。在下面的内容里,我们将会详细分析这三个过程的实现。

&emsp;在分析之前,要提醒读者注意的是, Spring把这三个过程分开,并使用不同的模块来完成,如使用相应的 Resourceloader、 Bean Definitionreader等模块,通过这样的设计方式,可以让用户更加灵活地对这三个过程进行剪裁或扩展,定义出最适合自己的IoC容器的初始化过程。

&emsp;第一个过程是 Resource'定位过程。这个 Resources定位指的是 Beandefinition的资源定位,它由 Resourceloader通过统一的 Resource:接口来完成,这个 Resource>对各种形式的Beandefinitiong的使用都提供了统一接口。对于这些 Beandefinition的存在形式,相信大家都不会感到陌生。比如,在文件系统中的Bean定义信息可以使用 Filesystemresource来进行抽象;在类路径中的Bean定义信息可以使用前面提到的 Classpathresource来使用,等等。这个定位过程类似于容器寻找数据的过程,就像用水桶装水先要把水找到一样。

&emsp;第二个过程是 Bean Definitiong的载入。这个载入过程是把用户定义好的Bean表示成oC容器内部的数据结构,而这个容器内部的数据结构就是 Beandefinition。下面介绍这个数据结构的详细定义。具体来说,这个 Beandefinition实际上就是POJO对象在IoC容器中的抽象,通过这个 Beandefinition'定义的数据结构,使IoC容器能够方便地对POJO对象也就是Bean进行管理。在下面的章节中,我们会对这个载入的过程进行详细的分析,使大家对整个过程有比较清楚的了解。

&emsp;第三个过程是向IoC容器注册这些 Bean Definitionl的过程。这个过程是通过调用Beandefinitionregistry接口的实现来完成的。这个注册过程把载入过程中解析得到的 Bean Definition向1oC容器进行注册。通过分析,我们可以看到,在IoC容器内部将 Bean Definition注入到个 Hashmap中去,IoC容器就是通过这个 Hashmap来持有这些 Beandefinition数据的。

&emsp;值得注意的是,这里谈的是IoC容器初始化过程,在这个过程中,一般不包含Bean依赖注人的实现。在 Spring Ioc的设计中,Bean定义的载入和依赖注入是两个独立的过程。依赖注入一般发生在应用第一次通过 getbeani向容器索取Bean的时候。但有一个例外值得注意,在使用IoC容器时有一个预实例化的配置,通过这个预实例化的配置(具体来说,可以通过为Bean定义信息中的 lazyinit属性),用户可以对容器初始化过程作一个微小的控制,从而改变这个被设置了 lazyinit属性的Bean的依赖注入过程。举例来说,如果我们对某个Bean设置了 azyinitk属性,那么这个Bean的依赖注入在IoC容器初始化时就预先完成了,而不需要等到整个初始化完成以后,第一次使用 get Beanl时才会触发。

&emsp;了解了IoC容器进行初始化的大致轮廓之后,下面我们详细地介绍在IoC容器的初始化过程中, Beandefinitionl的资源定位、载入和解析过程是怎么实现的。

#### 2.3.1 BeanDefinition的Resource定位

&emsp;以编程的方式使用 Defaultlistable Beanfactory时,首先定义一个 Resource来定位容器使用的 Beandefinition。这时使用的是 Class Pathresource,这意味着 Spring会在类路径中去寻找以文件形式存在的 Beandefinition信息。

```Java
ClassPathResource res = new ClassPathResource("beans.xml");
```

&emsp;这里定义的 Resource并不能由 Defaultlistablebean Factory直接使用, Spring通过Bean Definitionreader来对这些信息进行处理。在这里,我们也可以看到使用 ApplicationContext?相对于直接使用 Defaultlistable Bean Factory p的好处。因为在 Application Context.中,S prin g已经为我们提供了一系列加载不同 Resource的读取器的实现,而Defaultlistable Bean Factory只是一个纯粹的IoC容器,需要为它配置特定的读取器才能完成这些功能。当然,有利就有弊,使用 Defaultlistable Beanfactory这种更底层的容器,能提高定制IoC容器的灵活性。

&emsp;回到我们经常使用的 Application Context上来,例如 Filesystemxmlapplication Context、Classpath Xmlapplication Contextl以及 Xmlwebapplication Context等。简单地从这些类的名字上分析,可以清楚地看到它们可以提供哪些不同的 Resource读入功能,比如Filesystem Xmlapplication Context可以从文件系统载入 Resource, Classpath XmlapplicationContext可以从 Class Path载入 Resource, Xmlweb Applicationcontext可以在Web容器中载入Resource,等等。


&emsp;下面以 Filesystemxmlapplication Context为例,通过分析这个 Application Contexta的实现来看看它是怎样完成这个 Resource定位过程的。作为辅助,我们可以在图2-5中看到相应的Applicationcontext继承体系。


![FileSystemXmlApplicationContext的继承体系](http://q1q6z0e19.bkt.clouddn.com/1575203027347-3cfe88f1-68de-4847-86a5-e339d3d56a20.png)

&emsp;从源代码实现的角度,我们可以近距离关心以 Filesystemxmlapplication Conext为核心的继承体系,如图2-6所示。

![源代码角度的FileSystemXmlApplicationContext](http://q1q6z0e19.bkt.clouddn.com/FileSystemXmlApplicationContext.png)



#### 2.3.2 BeanDefinition的载入和解析


#### 2.3.3 BeanDefinition在IoC容器申的注册


### 2.4 IoC容器的依赖注入
### 2.5 容器其他相关特性的设计与实现
### 2.6 小结

## 第3章Spring AOP的实现

## 第4章　Spring MVC与Web环境

## 第5章　数据库操作组件的实现

## 第6章　Spring事务处理的实现

## 第7章　Spring远端调用的实现

## 第8章　安全框架ACEGI的设计与实现


## 第9章　Spring DM模块的设计与实现


## 第10章　Spring Flex的设计与实现
