# <center>《熟读丁雪丰译的《SpringBoot实战》</center>

---

## 第1章 入门

**本章内容**
* Spring Boot简化Spring应用程序开发
* Spring Boot的基本特性
* Spring Boot工作区的设置

&emsp;Spring Framework已有十余年的历史了，已成为Java应用程序开发框架的事实标准。在如此悠久的历史背景下，有人可能会认为Spring放慢了脚步，躺在了自己的荣誉簿上，再也做不出什么新鲜的东西，或者是让人激动的东西。甚至有人说， Spring是遗留项目，是时候去看看其他创新的东西了。

&emsp;这些人说得不对。

&emsp;Spring的生态圈里正在出现很多让人激动的新鲜事物，涉及的领域涵盖云计算、大数据、无模式的数据持久化、响应式编程以及客户端应用程序开发。


&emsp;在过去的一年多时间里，最让人兴奋、回头率最高、最能改变游戏规则的东西，大概就是SpringBoot了。 Spring Boot提供了一种新的编程范式，能在最小的阻力下开发Spring应用程序。有了它，你可以更加敏捷地开发Spring应用程序，专注于应用程序的功能，不用在Spring的配置上多花功夫，甚至完全不用配置。实际上， Spring Boot的一项重要工作就是让Spring不再成为你成功路上的绊脚石。

&emsp;本书将探索Spring Boot开发的诸多方面，但在开始前，我们先大概了解一下Spring Boot的功能。

### 1.1　Spring风云再起

&emsp;Spring诞生时是Java企业版（ Java Enterprise Edition， JEE，也称J2EE）的轻量级代替品。无需开发重量级的Enterprise JavaBean（ EJB）， Spring为企业级Java开发提供了一种相对简单的方法，通过依赖注入和面向切面编程，用简单的Java对象（ Plain Old Java Object， POJO）实现了EJB的功能。

&emsp;虽然Spring的组件代码是轻量级的，但它的配置却是重量级的。一开始， Spring用XML配置，而且是很多XML配置。 Spring 2.5引入了基于注解的组件扫描，这消除了大量针对应用程序自身组件的显式XML配置。 Spring 3.0引入了基于Java的配置，这是一种类型安全的可重构配置方式，可以代替XML。

&emsp;尽管如此，我们依旧没能逃脱配置的魔爪。开启某些Spring特性时，比如事务管理和Spring
MVC，还是需要用XML或Java进行显式配置。启用第三方库时也需要显式配置，比如基于Thymeleaf的Web视图。配置Servlet和过滤器（比如Spring的DispatcherServlet）同样需要在web.xml或Servlet初始化代码里进行显式配置。组件扫描减少了配置量， Java配置让它看上去简洁不少，但Spring还是需要不少配置。

&emsp;所有这些配置都代表了开发时的损耗。因为在思考Spring特性配置和解决业务问题之间需要进行思维切换，所以写配置挤占了写应用程序逻辑的时间。和所有框架一样， Spring实用，但与此同时它要求的回报也不少。

&emsp;除此之外，项目的依赖管理也是件吃力不讨好的事情。决定项目里要用哪些库就已经够让人头痛的了，你还要知道这些库的哪个版本和其他库不会有冲突，这难题实在太棘手。

&emsp;并且，依赖管理也是一种损耗，添加依赖不是写应用程序代码。一旦选错了依赖的版本，随之而来的不兼容问题毫无疑问会是生产力杀手。

&emsp;Spring Boot让这一切成为了过去。

#### 1.1.1　重新认识Spring

&emsp;假设你受命用Spring开发一个简单的Hello World Web应用程序。你该做什么？我能想到一些基本的需要。

* 一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件，最起码要有Spring MVC和Servlet API这些依赖。
* 一个web.xml文件（或者一个WebApplicationInitializer实现），其中声明了Spring的DispatcherServlet。
* 一个启用了Spring MVC的Spring配置。
* 一个控制器类，以“Hello World”响应HTTP请求。
* 一个用于部署应用程序的Web应用服务器，比如Tomcat。

&emsp;最让人难以接受的是，这份清单里只有一个东西是和Hello World功能相关的，即控制器，剩下的都是Spring开发的Web应用程序必需的通用样板。既然所有Spring Web应用程序都要用到它们，那为什么还要你来提供这些东西呢？

&emsp;假设这里只需要控制器。代码清单1-1所示基于Groovy的控制器类就是一个简单而完整的
Spring应用程序。

代码清单 1-1 一个完整的基于Groovy的Spring应用程序
```Java
@RestController
class HelloController {
    @RequestMapping("/")
    def hello() {
        return "Hello World"
    }
}
```

&emsp;这里没有配置，没有web.xml，没有构建说明，甚至没有应用服务器，但这就是整个应用程序了。 Spring Boot会搞定执行应用程序所需的各种后勤工作，你只要搞定应用程序的代码就好。

&emsp;假设你已经装好了Spring Boot的命令行界面（ Command Line Interface， CLI），可以像下面这样在命令行里运行HelloController：
```shell
spring run HelloController.groovy
```

想必你已经注意到了，这里甚至没有编译代码， Spring Boot CLI可以运行未经编译的代码。

&emsp;之所以选择用Groovy来写这个控制器示例，是因为Groovy语言的简洁与Spring Boot的简洁有异曲同工之妙。但Spring Boot并不强制要求使用Groovy。实际上，本书中的很多代码都是用Java写的，但在恰当的时候，偶尔也会出现一些Groovy代码。

&emsp;不要客气，直接跳到1.2.1节吧，看看如何安装Spring Boot CLI，这样你就能试着编写这个小小的Web应用程序了。现在，你将看到Spring Boot的关键部分，看到它是如何改变Spring应用程序的开发方式的。

#### 1.1.2　Spring Boot精要

&emsp;Spring Boot将很多魔法带入了Spring应用程序的开发之中，其中最重要的是以下四个核心。
* 自动配置：针对很多Spring应用程序常见的应用功能， Spring Boot能自动提供相关配置。
* 起步依赖：告诉Spring Boot需要什么功能，它就能引入需要的库。
* 命令行界面：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。
* Actuator：让你能够深入运行中的Spring Boot应用程序，一探究竟。

&emsp;每一个特性都在通过自己的方式简化Spring应用程序的开发。本书会探寻如何将它们发挥到极致，但就目前而言，先简单看看它们都提供了哪些功能吧。

&emsp;**1. 自动配置**
&emsp;在任何Spring应用程序的源代码里，你都会找到Java配置或XML配置（抑或两者皆有），它们为应用程序开启了特定的特性和功能。举个例子，如果你写过用JDBC访问关系型数据库的应用程序，那你一定在Spring应用程序上下文里配置过JdbcTemplate这个Bean。我打赌那段配置看起来是这样的：

```Java
@Bean
public JdbcTemplate jdbcTemplate(DataSource dataSource) {
    return new JdbcTemplate(dataSource);
}
```
这段非常简单的Bean声明创建了一个JdbcTemplate的实例，注入了一个DataSource依赖。当然，这意味着你还需要配置一个DataSource的Bean，这样才能满足依赖。假设你将配置一个嵌入式H2数据库作为DataSource Bean，完成这个配置场景的代码大概是这样的：

```Java
@Bean
public DataSource dataSource() {
    return new EmbeddedDatabaseBuilder()
        .setType(EmbeddedDatabaseType.H2)
        .addScripts('schema.sql', 'data.sql')
        .build();
}
```

这个Bean配置方法创建了一个嵌入式数据库，并指定在该数据库上执行两段SQL脚本。 build()方法返回了一个指向该数据库的引用。

&emsp;这两个Bean配置方法都不复杂，也不是很长，但它们只是典型Spring应用程序配置的一小部分。除此之外，还有无数Spring应用程序有着完全相同的方法。所有需要用到嵌入式数据库和JdbcTemplate的应用程序都会用到那些方法。简而言之，这就是一个样板配置。

&emsp;既然它如此常见，那为什么还要你去写呢？

&emsp;Spring Boot会为这些常见配置场景进行自动配置。如果Spring Boot在应用程序的Classpath里
发现H2数据库的库，那么它就自动配置一个嵌入式 H2 数据库。如果在Classpath里发现JdbcTemplate，那么它还会为你配置一个JdbcTemplate的Bean。你无需操心那些Bean的配置，Spring Boot会做好准备，随时都能将其注入到你的Bean里。

&emsp;Spring Boot的自动配置远不止嵌入式数据库和JdbcTemplate，它有大把的办法帮你减轻配
置负担，这些自动配置涉及Java持久化API（ Java Persistence API， JPA）、 Thymeleaf模板、安全和Spring MVC。第2章会深入讨论自动配置这个话题。

&emsp;**2. 起步依赖**
&emsp;向项目中添加依赖是件富有挑战的事。你需要什么库？它的Group和Artifact是什么？你需要
哪个版本？哪个版本不会和项目中的其他依赖发生冲突？

&emsp;Spring Boot通过起步依赖为项目的依赖管理提供帮助。起步依赖其实就是特殊的Maven依赖和Gradle依赖，利用了传递依赖解析，把常用库聚合在一起，组成了几个为特定功能而定制的依赖:

* org.springframework:spring-core
* org.springframework:spring-web
* org.springframework:spring-webmvc
* com.fasterxml.jackson.core:jackson-databind
* org.hibernate:hibernate-validator
* org.apache.tomcat.embed:tomcat-embed-core
* org.apache.tomcat.embed:tomcat-embed-el
* org.apache.tomcat.embed:tomcat-embed-logging-juli

&emsp;不过，如果打算利用Spring Boot的起步依赖，你只需添加Spring Boot的Web起步依赖
（ org.springframework.boot:spring-boot-starter-web），仅此一个。它会根据依赖
传递把其他所需依赖引入项目里，你都不用考虑它们。

&emsp;比起减少依赖数量，起步依赖还引入了一些微妙的变化。向项目中添加了Web起步依赖，实际上指定了应用程序所需的一类功能。因为应用是个Web应用程序，所以加入了Web起步依赖。与之类似如果应用程序要用到JPA持久化，那么就可以加入jpa起步依赖。如果需要安全功能，那就加入security起步依赖。简而言之，你不再需要考虑支持某种功能要用什么库了，引入相关起步依赖就行。

&emsp;此外， Spring Boot的起步依赖还把你从“需要这些库的哪些版本”这个问题里解放了出来。起步依赖引入的库的版本都是经过测试的，因此你可以完全放心，它们之间不会出现不兼容的情况。

&emsp;和自动配置一样，第2章就会深入讨论起步依赖。

&emsp;**3. 命令行界面**

&emsp;除了自动配置和起步依赖， Spring Boot还提供了一种很有意思的新方法，可以快速开发Spring应用程序。正如之前在1.1节里看到的那样， Spring Boot CLI让只写代码即可实现应用程序成为可能。

&emsp;Spring Boot CLI利用了起步依赖和自动配置，让你专注于代码本身。不仅如此，你是否注意
到代码清单1-1里没有import？ CLI如何知道RequestMapping和RestController来自哪个包呢？说到这个问题，那些类最终又是怎么跑到Classpath里的呢？

&emsp;说得简单一点， CLI能检测到你使用了哪些类，它知道要向Classpath中添加哪些起步依赖才
能让它运转起来。一旦那些依赖出现在Classpath中，一系列自动配置就会接踵而来，确保启用
DispatcherServlet和Spring MVC，这样控制器就能响应HTTP请求了。

&emsp;Spring Boot CLI是Spring Boot的非必要组成部分。虽然它为Spring带来了惊人的力量，大大简化了开发，但也引入了一套不太常规的开发模型。要是这种开发模型与你的口味相去甚远，那也没关系，抛开CLI，你还是可以利用Spring Boot提供的其他东西。不过如果喜欢CLI，你一定想看看第5章，其中深入探讨了Spring Boot CLI。

&emsp;**4. Actuator**

&emsp;Spring Boot的最后一块“拼图”是Actuator，其他几个部分旨在简化Spring开发，而Actuator则要提供在运行时检视应用程序内部情况的能力。安装了Actuator就能窥探应用程序的内部情况了，包括如下细节：

* Spring应用程序上下文里配置的Bean
* Spring Boot的自动配置做的决策
* 应用程序取到的环境变量、系统属性、配置属性和命令行参数
* 应用程序里线程的当前状态
* 应用程序最近处理过的HTTP请求的追踪情况
* 各种和内存用量、垃圾回收、 Web请求以及数据源用量相关的指标

&emsp;Actuator通过Web端点和shell界面向外界提供信息。如果要借助shell界面，你可以打开SSH
（ Secure Shell），登入运行中的应用程序，发送指令查看它的情况。

&emsp;第7章会详细探索Actuator的功能。


#### 1.1.3　Spring Boot不是什么

&emsp;因为Spring Boot实在是太惊艳了，所以过去一年多的时间里有不少和它相关的言论。原先听
到或看到的东西可能给你造成了一些误解，继续学习本书前应该先澄清这些误会。

&emsp;首先， Spring Boot不是应用服务器。这个误解是这样产生的： Spring Boot可以把Web应用程序变为可自执行的JAR文件，不用部署到传统Java应用服务器里就能在命令行里运行。 Spring Boot在应用程序里嵌入了一个Servlet容器（ Tomcat、 Jetty或Undertow），以此实现这一功能。但这是内嵌的Servlet容器提供的功能，不是Spring Boot实现的。

&emsp;与之类似， Spring Boot也没有实现诸如JPA或JMS（ Java Message Service， Java消息服务）之类的企业级Java规范。它的确支持不少企业级Java规范，但是要在Spring里自动配置支持那些特性的Bean。例如， Spring Boot没有实现JPA，不过它自动配置了某个JPA实现（比如Hibernate）的Bean，以此支持JPA。

&emsp;最后， Spring Boot没有引入任何形式的代码生成，而是利用了Spring 4的条件化配置特性，以及Maven和Gradle提供的传递依赖解析，以此实现Spring应用程序上下文里的自动配置。

&emsp;简而言之，从本质上来说， Spring Boot就是Spring，它做了那些没有它你自己也会去做的Spring Bean配置。谢天谢地，幸好有Spring，你不用再写这些样板配置了，可以专注于应用程序的逻辑，这些才是应用程序独一无二的东西。


&emsp;简而言之，从本质上来说， Spring Boot就是Spring，它做了那些没有它你自己也会去做的Spring Bean配置。谢天谢地，幸好有Spring，你不用再写这些样板配置了，可以专注于应用程序的逻辑，这些才是应用程序独一无二的东西。


### 1.2　Spring Boot入门

&emsp;从根本上来说， Spring Boot的项目只是普通的Spring项目，只是它们正好用到了Spring Boot的起步依赖和自动配置而已。因此，那些你早已熟悉的从头创建Spring项目的技术或工具，都能
用于Spring Boot项目。然而，还是有一些简便的途径可以用来开启一个新的Spring Boot项目。

&emsp;最快的方法就是安装Spring Boot CLI，安装后就可以开始写代码，如代码清单1-1，接着通过CLI来运行就好。

#### 1.2.1　安装Spring Boot CLI


&emsp;如前文所述， Spring Boot CLI提供了一种有趣的、不同寻常的Spring应用程序开发方式。第5章里会详细解释CLI提供的功能。这里先来看看如何安装Spring Boot CLI，这样才能运行代码清
单1-1。

&emsp;Spring Boot CLI有好几种安装方式。

* 用下载的分发包进行安装。
* 用Groovy Environment Manager进行安装。
* 通过OS X Homebrew进行安装。
* 使用MacPorts进行安装。

我们分别看一下这几种方式。除此之外，还要了解如何安装Spring Boot CLI的命令行补全支持，
如果你在BASH或zsh shell里使用CLI，这会非常有用（抱歉了，各位Windows用户）。先来看看如
何用分发包手工安装Spring Boot CLI吧。

&emsp;**1. 手工安装Spring Boot CLI**

&emsp;安装Spring Boot CLI最直接的方法大约是下载、 解压， 随后将它的bin目录添加到系统路径里。
你可以从以下两个地址下载分发包：
* http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.0.RELEASE/springboot-cli-1.3.0.RELEASE-bin.zip
* http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.0.RELEASE/springboot-cli-1.3.0.RELEASE-bin.tar.gz

&emsp;下载完成之后，把它解压到文件系统的任意目录里。在解压后的目录里，你会找到一个bin目录，其中包含了一个spring.bat脚本（用于Windows环境）和一个spring脚本（用于Unix环境）。把这个bin目录添加到系统路径里，然后就能使用Spring Boot CLI了。

&emsp;&emsp;**为Spring Boot建立符号链接** &emsp;如果是在安装了Unix的机器上使用Spring Boot CLI，最好建立一个指向解压目录的符号链接，然后把这个符号链接添加到系统路径，而不是实际的目录。这样后续升级Spring Boot新版本，或是转换版本，都会很方便，只要重建一下符号链接，指向新版本就好了。

&emsp;你可以先浅尝辄止，看看你所安装的CLI版本号：

```shell
$ spring --version
```

如果一切正常，你会看到安装好的Spring Boot CLI的版本号。

&emsp;虽然这是手工安装，但一切都很容易，而且不要求你安装任何附加的东西。如果你是Windows用户，也别无选择，这是唯一的安装方式。但如果你使用的是Unix机器，而且想要稍微自动化一点的方式，那么可以试试Software Development Kit Manager。


&emsp;**2. 使用Software Development Kit Manager进行安装**

&emsp;软件开发工具管理包（ Software Development Kit Manager， SDKMAN，曾用简称GVM）也能用来安装和管理多版本Spring Boot CLI。使用前，你需要先从http://sdkman.io获取并安SDKMAN。最简单的安装方式是使用命令行：

```shell
$ curl -s get.sdkman.io | bash
```
跟随输出的指示就能完成SDKMAN的安装。在我的机器上，我在命令行里执行了如下命令：
```shell
$ source "/Users/habuma/.sdkman/bin/sdkman-init.sh"
```

&emsp;注意，用户不同，这条命令也会有所不同。我的用户目录是/Users/habuma，因此这也是shell脚本的根路径。你需要根据实际情况稍作调整。一旦安装好了SDKMAN，就可以用下面的方式来安装Spring Boot CLI了：

```shell
$ sdk install springboot
$ spring --version
```
假设一切正常，你将看到Spring Boot的当前版本号。

&emsp;如果想升级新版本的Spring Boot CLI，只需安装并使用即可。使用SDKMAN的list命令可以找到可用的版本：

```shell
$ sdk list springboot
```

list命令列出了所有可用版本，包括已经安装的和正在使用的。从中选择一个进行安装，然后就可以正常使用。举例来说，要安装Spring Boot CLI 1.3.0.RELEASE，直接使用install命令，指定版本号：

```shell
$ sdk install springboot 1.3.0.RELEASE
```
这样就会安装一个新版本，随后你会被询问是否将其设置为默认版本。要是你不想把它作为默认版本，或者想要切换到另一个版本，可以用use命令：

```shell
$ sdk use springboot 1.3.0.RELEASE
```
&emsp;如果你希望把那个版本作为所有shell的默认版本，可以使用default命令：

```shell
$ sdk default springboot 1.3.0.RELEASE
```

&emsp;使用SDKMAN来管理Spring Boot CLI有一个好处，你可以便捷地在Spring Boot的不同版本之间切换。这样你可以在正式发布前试用快照版本（ snapshot）、里程碑版本（ milestone）和尚未正式发布的候选版本（ release candidate），试用后再切回稳定版本进行其他工作。

&emsp;**3. 使用Homebrew进行安装**

&emsp;如果要在OS X的机器上进行开发，你还可以用Homebrew来安装Spring Boot CLI。 Homebrew是OS X的包管理器，用于安装多种不同应用程序和工具。要安装Homebrew，最简单的方法就是运行安装用的Ruby脚本：

```shell
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

&emsp;你可以在http://brew.sh看到更多关于Homebrew的内容（还有安装方法）。

&emsp;要用Homebrew来安装Spring Boot CLI，你需要引入Pivotal的tap：

```shell
$ brew tap pivotal/tap
```
&emsp;在有了Pivotal的tap后，就可以像下面这样安装Spring Boot CLI了：

```shell
$ brew install springboot
```

&emsp;Homebrew会把Spring Boot CLI安装到/usr/local/bin，之后可以直接使用。可以通过检查版本号来验证安装是否成功：

```shell
$ spring --version
```

&emsp;这条命令应该会返回刚才安装的Spring Boot版本号。你也可以运行代码清单1-1看看。

#### 1.2.2　使用Spring Initializr初始化Spring Boot项目

### 1.3　小结



## 第2章    开发第一个应用程序 

## 第3章    自定义配置

### 3.1　覆盖Spring Boot自动配置

&emsp;一般来说，如果不用配置就能得到和显式配置一样的结果，那么不写配置是最直接的选择。既然如此，那干嘛还要多做额外的工作呢？如果不用编写和维护额外的配置代码也行，那何必还要它们呢？

&emsp;大多数情况下，自动配置的Bean刚好能满足你的需要，不需要去覆盖它们。但某些情况下，Spring Boot在自动配置时还不能很好地进行推断。

&emsp;这里有个不错的例子：当你在应用程序里添加安全特性时，自动配置做得还不够好。安全配置并不是放之四海而皆准的，围绕应用程序安全有很多决策要做， Spring Boot不能替你做决定。虽然Spring Boot为安全提供了一些基本的自动配置，但是你还是需要自己覆盖一些配置以满足特定的安全要求。

&emsp;想知道如何用显式的配置来覆盖自动配置，我们先从为阅读列表应用程序添加Spring Security入手。在了解自动配置提供了什么之后，我们再来覆盖基础的安全配置，以满足特定的场景需求。

#### 3.1.1　保护应用程序

&emsp;Spring Boot自动配置让应用程序的安全工作变得易如反掌，你要做的只是添加Security起步依赖。以Gradle为例，应添加如下依赖：
```Java
compile("org.springframework.boot:spring-boot-starter-security")
```

&emsp;如果使用Maven，那么你要在项目的<dependencies>块中加入如下<dependency>：

```Xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

&emsp;这样就搞定了！重新构建应用程序后运行即可，现在这就是一个安全的Web应用程序了！Security起步依赖在应用程序的Classpath里添加了Spring Secuirty（和其他一些东西）。 Classpath里有Spring Security后，自动配置就能介入其中创建一个基本的Spring Security配置。

&emsp;试着在浏览器里打开该应用程序，你马上就会看到HTTP基础身份验证对话框。此处的用户名是user，密码就有点麻烦了。密码是在应用程序每次运行时随机生成后写入日志的，你需要查找日志消息（默认写入标准输出），找到此类内容：

```yml
Using default security password: d9d8abe5-42b5-4f20-a32a-76ee3df658d9
```

&emsp;我不能肯定，但我猜这个特定的安全配置并不是你的理想选择。首先， HTTP基础身份验证对话框有点粗糙，对用户并不友好。而且，我敢打赌你一般不会开发这种只有一个用户的应用程序，而且他还要从日志文件里找到自己的密码。因此，你会希望修改Spring Security的一些配置，至少要有一个好看一些的登录页，还要有一个基于数据库或LDAP（ Lightweight Directory AccessProtocol）用户存储的身份验证服务。

&emsp;让我们看看如何写出Spring Secuirty配置，覆盖自动配置的安全设置吧。


#### 3.1.2　创建自定义的安全配置

&emsp;覆盖自动配置很简单，就当自动配置不存在，直接显式地写一段配置。这段显式配置的形式不限， Spring支持的XML和Groovy形式配置都可以。

&emsp;在编写显式配置时，我们会专注于Java形式的配置。在Spring Security的场景下，这意味着写一个扩展了WebSecurityConfigurerAdapter的配置类。代码清单3-1中的SecurityConfig就是我们需要的东西。

代码清单3-1 覆盖自动配置的显式安全配置
```Java
package readinglist;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private ReaderRepository readerRepository;
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/").access("hasRole('READER')") //要求登录者有READER角色
            .antMatchers("/**").permitAll()
            .and()
            .formLogin()
            .loginPage("/login") //设置登录表单的路径
            .failureUrl("/login?error=true");
    }

    @Override
    protected void configure(
    AuthenticationManagerBuilder auth) throws Exception {
        //定义自定义: UserDetailsService
        auth.userDetailsService(new UserDetailsService() {
            @Override
            public UserDetails loadUserByUsername(String username)throws UsernameNotFoundException {
                return readerRepository.findOne(username);
            }
        });
    }
}
```
&emsp;SecurityConfig是个非常基础的Spring Security配置，尽管如此，它还是完成了不少安全定制工作。通过这个自定义的安全配置类，我们让Spring Boot跳过了安全自动配置，转而使用我们的安全配置。

&emsp;扩展了WebSecurityConfigurerAdapter的配置类可以覆盖两个不同的configure()方法。在SecurityConfig里，第一个configure()方法指明， “/”（ ReadingListController的方法映射到了该路径）的请求只有经过身份认证且拥有READER角色的用户才能访问。其他的所有请求路径向所有用户开放了访问权限。这里还将登录页和登录失败页（带有一个error属性）指定到了/login。

&emsp;Spring Security为身份认证提供了众多选项，后端可以是JDBC（ Java Database Connectivity）、LDAP和内存用户存储。在这个应用程序中，我们会通过JPA用数据库来存储用户信息。第二个configure()方法设置了一个自定义的UserDetailsService，这个服务可以是任意实现了UserDetailsService的类，用于查找指定用户名的用户。代码清单3-2提供了一个匿名内部类实现，简单地调用了注入ReaderRepository（这是一个Spring Data JPA仓库接口）的findOne()方法。

代码清单3-2 用来持久化读者信息的仓库接口
```Java
package readinglist;
import org.springframework.data.jpa.repository.JpaRepository;
public interface ReaderRepository extends JpaRepository<Reader, String> {

    //通过JPA持久化读者
}
```


&emsp;和BookRepository类似，你无需自己实现ReaderRepository。 这是因为它扩展了JpaRepository， Spring Data JPA会在运行时自动创建它的实现。这为你提供了18个操作Reader实体的方法。

&emsp;说到Reader实体， Reader类（如代码清单3-3所示）就是最后一块拼图了，它就是一个简单的JPA实体，其中有几个字段用来存储用户名、密码和用户全名。

代码清单3-3 定义Reader的JPA实体

```Java
package readinglist;
import java.util.Arrays;
import java.util.Collection;
import javax.persistence.Entity;
import javax.persistence.Id;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
@Entity
public class Reader implements UserDetails {
    private static final long serialVersionUID = 1L;
    @Id
    private String username;
    private String fullname;
    private String password;
    public String getUsername() {
        return username;
    }
    public void setUsername(String username) {
        this.username = username;
    }
    public String getFullname() {
        return fullname;
    }
    public void setFullname(String fullname) {
        this.fullname = fullname;
    }
    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }
    // UserDetails methods
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return Arrays.asList(new SimpleGrantedAuthority("READER"));
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }
    @Override
    public boolean isAccountNonLocked() {
        return true;
    }
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }
    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

&emsp;如你所见， Reader用了@Entity注解，所以这是一个JPA实体。此外，它的username字段上有@Id注解，表明这是实体的ID。这个选择无可厚非，因为username应该能唯一标识一个Reader。

&emsp;你应该还注意到Reader实现了UserDetails接口以及其中的方法，这样Reader就能代表Spring Security里的用户了。 getAuthorities()方法被覆盖过了，始终会为用户授予READER权限。 isAccountNonExpired()、 isAccountNonLocked()、 isCredentialsNonExpired()和isEnabled()方法都返回true，这样读者账户就不会过期，不会被锁定，也不会被撤销。

&emsp;重新构建并重启应用程序后，你应该就能以读者身份登录应用程序了。

&emsp;**保持简单** &emsp;在一个大型应用程序里，赋予用户的授权本身也可能是实体，它们被维护在独立的数据表里。同样，表示一个账户是否为非过期、非锁定且可用的布尔值也是数据库里的字段。但是，出于演示考虑，我决定让这些细节保持简单，以免分散我们的注意力，影响正在讨论的话题——我说的是覆盖Spring Boot自动配置。

&emsp;在安全配置方面，我们还能做更多事情，但此刻这样就足够了，上面的例子足以演示如何覆盖Spring Boot提供的安全自动配置。

&emsp;再重申一次，想要覆盖Spring Boot的自动配置，你所要做的仅仅是编写一个显式的配置。Spring Boot会发现你的配置，随后降低自动配置的优先级，以你的配置为准。想弄明白这是如何实现的，让我们揭开Spring Boot自动配置的神秘面纱，看看它是如何运作的，以及它是怎么允许自己被覆盖的。

#### 3.1.3　掀开自动配置的神秘面纱

&emsp;正如我们在2.3.3节里讨论的那样， Spring Boot自动配置自带了很多配置类，每一个都能运用在你的应用程序里。它们都使用了Spring 4.0的条件化配置，可以在运行时判断这个配置是该被运用，还是该被忽略。

&emsp;大部分情况下，表2-1里的@ConditionalOnMissingBean注解是覆盖自动配置的关键。Spring Boot的DataSourceAutoConfiguration中定义的JdbcTemplate Bean就是一个非常简单的例子，演示了@ConditionalOnMissingBean如何工作：

```Java
@Bean
@ConditionalOnMissingBean(JdbcOperations.class)
public JdbcTemplate jdbcTemplate() {
    return new JdbcTemplate(this.dataSource);
}
```

&emsp;jdbcTemplate()方法上添加了@Bean注解，在需要时可以配置出一个JdbcTemplate Bean。但它上面还加了@ConditionalOnMissingBean注解，要求当前不存在JdbcOperations类型（ JdbcTemplate实现了该接口）的Bean时才生效。如果当前已经有一个JdbcOperations Bean了，条件即不满足，不会执行jdbcTemplate()方法。

&emsp;什么情况下会存在一个JdbcOperations Bean呢？ Spring Boot的设计是加载应用级配置，随后再考虑自动配置类。因此，如果你已经配置了一个JdbcTemplate Bean，那么在执行自动配置时就已经存在一个JdbcOperations类型的Bean了，于是忽略自动配置的JdbcTemplate Bean。

&emsp;关于Spring Security，自动配置会考虑几个配置类。在这里讨论每个配置类的细节是不切实际的， 但覆盖Spring Boot自动配置的安全配置时，最重要的一个类是SpringBootWebSecurityConfiguration。以下是其中的一个代码片段：




### 3.2　通过属性文件外置配置
#### 3.2.1　自动配置微调
#### 3.2.2　应用程序Bean的配置外置
#### 3.2.3　使用Profile进行配置
### 3.3　定制应用程序错误页面
### 3.4　小结

## 第4章    测试

## 第5章    Groovy与Spring Boot CLI

## 第6章    在Spring Boot中使用Grails

## 第7章    深入Actuator

## 第8章    部署Spring Boot应用程序