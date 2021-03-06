### Java中的IO原理

首先Java中的IO都是依赖操作系统内核进行的，我们程序中的IO读写其实调用的是操作系统内核中的read&write两大系统调用。

### 那内核是如何进行IO交互的呢？

1. 网卡收到经过网线传来的网络数据，并将网络数据写到内存中。
2. 当网卡把数据写入到内存后，网卡向cpu发出一个中断信号，操作系统便能得知有新数据到来，再通过网卡中断程序去处理数据。 
3. 将内存中的网络数据写入到对应socket的接收缓冲区中。
4. 当接收缓冲区的数据写好之后，应用程序开始进行数据处理。
### 同步与异步

**同步：**

同步指的是调用一旦开始，调用者必须等到方法调用返回后，才能继续后续的行为。即方法二一定要等到方法一执行完成后才可以执行。

**异步：**

异步指的是调用立刻返回，调用者不必等待方法内的代码执行结束，就可以继续后续的行为。

### 阻塞与非阻塞

阻塞：

阻塞指的是遇到同步等待后，一直在原地等待同步方法处理完成。

注：同步与异步关注的是方法的执行方是主线程还是其他线程，主线程的话需要等待方法执行完成，其他线程的话无需等待立刻返回方法调用，主线程可以直接执行接下来的代码。

非阻塞：

非阻塞指的是遇到同步等待，不在原地等待，先去做其他的操作，隔断时间再来观察同步方法是否完成。

注：阻塞与非阻塞关注的是线程是否在原地等待。

### 例子讲解：

* 顾客去吃海底捞，就这样干坐着等了一小时，然后才开始吃火锅。(BIO)
* 顾客去吃海底捞，他一看要等挺久，于是去逛商场，每次逛一会就跑回来看有没有排到他。于是他最后既购了物，又吃上海底捞了。（NIO）
* 顾客去吃海底捞，由于他是高级会员，所以店长说，你去商场随便玩吧，等下有位置，我立马打电话给你。于是C顾客不用干坐着等，也不用每过一会儿就跑回来看有没有等到，最后也吃上了海底捞（AIO）
### BIO

** 同步阻塞模式**。线程发起IO请求后，一直阻塞IO，直到缓冲区数据就绪后，再进入下一步操作。

#### NIO

同步非阻塞的IO模型。线程发起io请求后，立即返回（非阻塞io）。同步指的是必须等待IO缓冲区内的数据就绪，而非阻塞指的是，用户线程不原地等待IO缓冲区，可以先做一些其他操作，但是要定时轮询检查IO缓冲区数据是否就绪。

#### AIO

异步非阻塞IO模型。异步非阻塞IO应该让内核系统完成，用户线程只需要告诉内核，当缓冲区就绪后，通知我或者执行我交给你的回调函数。

