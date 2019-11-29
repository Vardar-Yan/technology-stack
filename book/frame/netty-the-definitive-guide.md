
## 资源下载
>[PDF下载](https://pan.baidu.com/s/1eSz3U0Pm7EeL1q-bU3diUw)

---

## 第1章 Java的I/O演进之路


### 1.1 I/O基础入门

#### 1.1.1 Linux网络I/O模型简介
#### 1.1.2 I/O多路复用技术

### 1.2 Java的I/O演进

### 1.3 总结

## 第2章 NIO入门

&emsp;在本章中，我们会分别对JDK的BIO、NIO和JDK1.7最新提供的NIO2.0的使用进行详细说明，通过流程图和代码讲解，让大家体会到随着Java I/O类库的不断发展和改进，基于Java的网络编程会变得越来越简单，随着一步I/O功能的增强，基于Java NIO开发的网络服务器甚至不逊色于采用C++开发的网络程序。
&emsp;本章主要内容包括：
* 传统的同步阻塞式I/O编程
* 基于NIO的非阻塞编程
* 基于NIO2.0的一步非阻塞（AIO）编程
* 为什么选择Netty

### 2.1 传统的BIO编程
&emsp;网络编程的基本模型是Client/Server模型，也就是两个进程之间进行相互通信，其中服务端提供位置信息（绑定的IP地址和监听端口），客户端通过连接操作向服务端听的地址发起连接请求，通过三次握手建立连接，如果连接建立成功，双方就可以通过网络套接字（Socket）进行通信。
&emsp;在基于传统同步阻塞模型开发中，ServerSocket负责绑定IP地址，启动监听端口；Socket负责发起连接操作。连接成功之后，双方通过输入和输出流进行同步阻塞式通信。
&emsp;下面，我们就以经典的时间服务器（TimeServer）为例，通过代码分析来回顾和熟悉下BIO编程。
 

#### 2.1.1 BIO通信模型图
#### 2.1.2 同步阻塞式I/O创建的TimeServer源码分析
#### 2.1.3 同步阻塞式I/O创建的TimeClient源码分析
### 2.2 伪异步I/O编程
#### 2.2.1 伪异步I/O模型图
#### 2.2.2 伪异步式I/O创建的TimeServer源码分析
#### 2.2.3 伪异步I/O弊端分析
### 2.3 NIO编程
#### 2.3.1 NIO类库简介
#### 2.3.2 NIO服务端序列图
#### 2.3.3 NIO创建的TimeServer源码分析
#### 2.3.4 NIO客户端序列图
#### 2.3.5 NIO创建的TimeClient源码分析
### 2.4 AIO编程
#### 2.4.1 AIO创建的TimeServer源码分析
#### 2.4.2 AIO创建的TimeClient源码分析
#### 2.4.3 AIO版本时间服务器运行结果
### 2.5 4种I/O的对比
#### 2.5.1 概念澄清
#### 2.5.2 不同I/O模型对比
### 2.6 选择Netty的理由
#### 2.6.1 不选择Java原生NIO编程的原因
#### 2.6.2 为什么选择Netty
### 2.7 总结

## 第3章 Netty入门应用
&emsp;作为Nety的第一个应用程序,我们依然以第2章的时间服务器为例进行开发,通过Nety版本的时间服务器的开发,让初学者尽快学到如何搭建Nety开发环境和运行 Netty应用程序。

&emsp;如果你已经熟悉Nety的基础应用,可以跳过本章,继续后面知识

&emsp;识的学习本章主要内容包括:
* Netty开发环境的搭建
* 服务端程序TimeServer开发
* 客户端程序TimeClient开发
* 时间服务器的运行和调试

### 3.1 Netty开发环境的搭建

&emsp;首先假设你已经在本机安装了JDK1.7,配置了JDK的环境变量path,同时下载并正确启动了IDE工具 Eclipse。如果你是个Java初学者,从来没有在本机搭建过Java开发环境,建议你先选择一本Java基础入门的书籍或者课程进行学习。

&emsp;假如你习惯于使用其他IDE工具进行Java开发,例如 Netbeans IDE,也可以运行本节的入门例程。但是,你需要根据自己实际使用的IDE进行对应的配置修改和调整,本书统一使用 eclipse-jee- kepler-SRI-win32作为Java开发工具。下面我们开始学习如何搭建 Netty的开发环境。

#### 3.1.1 下载Netty的软件包

&emsp;访问Nety的官网http://netty.io/,从【 Downloads】标签页选择下载5.0.0. Alphal安装包,安装包不大,8.95M左右。

#### 3.1.2 搭建Netty应用工程
### 3.2 Netty服务端开发

&emsp;作为第一个Nety的应用例程,为了让读者能够将精力集中在Nety的使用上,我们依然选择第2章的时间服务器为例进行源码开发和代码讲解。

&emsp;**Time Server开发**

&emsp;在开始使用Nety开发 Timeserver之前,先回顾一下使用NIO进行服务端开发的步骤。

&emsp;(1)创建 Serversocket Channel,配置它为非阻塞模式;

&emsp;(2)绑定监听,配置TCP参数,例如 backlog大小

&emsp;(3)创建一个独立的I/O线程,用于轮询多路复用器 Selector

&emsp;(4)创建 Selector,将之前创建的 ServersocketChannel注册到 Selector上,监听Selection Key.ACCEPT;

&emsp;(5)启动IO线程,在循环体中执行 Selector.select()方法,轮询就绪的 Channel;

&emsp;(6)当轮询到了处于就绪状态的 Channel时,需要对其进行判断,如果是 OP_ACCEPT状态,说明是新的客户端接入,则调用 ServersocketChannel.accept()方法接受新的客户端;

&emsp;(7)设置新接入的客户端链路 SocketChannel为非阻塞模式,配置其他的一些TCP参数;

&emsp;(8)将 SocketChannel注册到 Selector,监听 OP_READ操作位;

&emsp;(9)如果轮询的 Channel为 OP_READ,则说明 SocketChannel中有新的就绪的数据包需要读取,则构造 ByteBuffer对象,读取数据包;

&emsp;(10)如果轮询的 Channel为 OP_WRITE,说明还有数据没有发送完成,需要继续发送。

&emsp;一个简单的NIO服务端程序,如果我们直接使用JDK的NIO类库进行开发,竟然需要经过烦琐的十多步操作才能完成最基本的消息读取和发送,这也是我们要选择Nety等NIO框架的原因了,下面我们看看使用 Netty是如何轻松搞定服务端开发的。

<center>代码清单3-1 Netty时间服务器服务端TimeServer</center>

```Java

```
&emsp;由于本章的重点是讲解 Netty的应用开发,所以对于一些Net的类库和用法仅仅做基础性的讲解,我们从黑盒的角度理解这些概念即可。后续源码分析章节会专门对 Netty核心的类库和功能进行分析,感兴趣的同学可以跳到源码分析章节进行后续的学习。

&emsp;我们从bind方法开始学习,在代码第20~21行创建了两个 Nioevent Group实例。Nioeventloopgroup是个线程组,它包含了一组NIO线程,专门用于网络事件的处理,实际上它们就是 Reactor线程组。这里创建两个的原因是一个用于服务端接受客户端的连接,另一个用于进行 Socket Channel的网络读写。第23行我们创建 Serverbootstrap对象,它是Nety用于启动NIO服务端的辅助启动类,目的是降低服务端的开发复杂度。第24行调用Serverbootstrap的 group方法,将两个NIO线程组当作入参传递到 Server Bootstrap中。接着设置创建的 Channel为 Nioserversocket Channel,它的功能对应于 JDK NIO类库中的Serversocket Channel类。然后配置 Nioserversocket Channel I的TCP参数,此处将它的 backlog设置为1024,最后绑定I/O事件的处理类 Childchannelhandler,它的作用类似于 Reactor模式中的 handler类,主要用于处理网络I/O事件,例如记录日志、对消息进行编解码等。

&emsp;服务端启动辅助类配置完成之后,调用它的bind方法绑定监听端口,随后,调用它的同步阻塞方法sync等待绑定操作完成。完成之后Nety会返回一个 Channelfuture,它的功能类似于JDK的java.util. concurrent. Future,主要用于异步操作的通知回调。

&emsp;第32行使用f.channel().closeFuture().sync()方法进行阻塞,等待服务端链路关闭之后main函数才退出。

&emsp;第34~36行调用NIO线程组的 shutdowngracefully进行优雅退出,它会释放跟shutdownGracefully相关联的资源。

<center>代码清单3-2 Netty时间服务器服务端TimeServerHandler</center>

```Java

```
&emsp;TimeserverHandler继承自 ChannelHandlerAdapter,它用于对网络事件进行读写操作,通常我们只需要关注 channelRead和 exceptionCaught方法。下面对这两个方法进行简单说明。

&emsp;第17行做类型转换,将msg转换成 Netty的 ByteBuf对象。 ByteBuf类似于JDK中的ava.nio.ByteBuffer对象,不过它提供了更加强大和灵活的功能。通过 Bytebuf的readableBytes方法可以获取缓冲区可读的字节数,根据可读的字节数创建byte数组,通过Bytebuf的 readbytes方法将缓冲区中的字节数组复制到新建的byte数组中,最后通过newString构造函数获取请求消息。这时对请求消息进行判断,如果是" QUERY TIME ORDER"则创建应答消息,通过 ChannelhandlerContext的write方法异步发送应答消息给客户端。

&emsp;第30行我们发现还调用了 ChannelhandlerContext的fush方法,它的作用是将消息发送队列中的消息写入到 SocketChannel中发送给对方。从性能角度考虑,为了防止频繁地唤醒Selector进行消息发送, Nety的 write方法并不直接将消息写入 SocketChannel I中调用 write方法只是把待发送的消息放到发送缓冲数组中,再通过调用fush方法,将发送缓冲区中的消息全部写到 SocketChannel中。

&emsp;第35行,当发生异常时,关闭 ChannelhandlerContext,释放和 ChannelhandlerContext相关联的句柄等资源。

&emsp;通过对代码进行统计分析可以看出,不到30行的业务逻辑代码,即完成了NIO服务端的开发,相比于传统基于 JDK NIO原生类库的服务端,代码量大大减少,开发难度也降低了很多。

&emsp;下面我们继续学习客户端的开发,并使用 Netty改造TimeClient。

### 3.3 Netty客户端开发

&emsp;Netty客户端的开发相比于服务端更简单,下面我们就看下客户端的代码如何实现。

&emsp;**TimeClient开发**

<center>代码经典3-3 Netty时间服务器客户端 TimeClient</center>

```Java

```

&emsp;我们从 connect方法讲起,在第20行首先创建客户端处理IO读写的 NioeventloopGroup线程组,然后继续创建客户端辅助启动类 Bootstrap,随后需要对其进行配置。与服务端不同的是,它的 Channel需要设置为 Niosocket Channel,然后为其添加 handler,此处为了简单直接创建匿名内部类,实现 init Channel方法,其作用是当创建 NiosocketChannel成功之后,在初始化它的时候将它的 Channelhandler设置到 Channelpipeline中,用于处理网络I/O事件。



### 3.4 运行和调试
#### 3.4.1 服务端和客户端的运行
#### 3.4.2 打包和部署
### 3.5 总结

## 第4章 TCP粘包/拆包问题的解决之道

## 第5章 分隔符和定长解码器的应用

## 第6章 编解码技术

## 第7章 Java序列化

## 第8章 Google Protobuf编解码

## 第9章 JBoss Marshalling编解码

## 第10章 HTTP协议开发应用

## 第11章 WebSocket协议开发

## 第12章 UDP协议开发

## 第13章 文件传输

## 第14章 私有协议栈开发

## 第15章 ByteBuf和相关辅助类

## 第16章 Channel和Unsafe

## 第17章 ChannelPipeline和ChannelHandler

## 第18章 EventLoop和EventLoopGroup

## 第19章 Future和Promise

## 第20章 Java多线程编程在Netty中的应用

## 第21章 Netty架构剖析

## 第22章 Netty行业应用

## 第23章 Netty未来展望