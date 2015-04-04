为何使用 MINA
====

写网络应用常常被视作一种高负担但低水平的开发。这是一个不经常为程序员所学习或者了解的领域，这可能是因为这些内容是在很久以前在学校里学过但都忘光了，也可能是因为这一网络层的复杂性常常被更高层的传输层所隐藏以致你从来没有深入它。
       
补充一点，当涉及到异步 IO 时，一个额外的复杂的层出场了：时间。

BIO (Blocking IO，阻塞 IO) 和 NIO (Non-Blocking IO，非阻塞 IO) 之间的最大的区别在于在 BIO 中，你发送了一个请求，然后你将在得到回复之前一直等待。在服务器端，这意味着一个线程可能会涉及到任何进入的连接，因此你不需要应对多路复用连接的复杂性。另一方面，在 NIO 中，你必须应对非阻塞系统的同步特性，这意味着在一些事件发生时你的应用会被调用。在 NIO 中，你无需调用了以后等待一个结果，你发送一条命令之后，结果就绪了你会被通知。

##框架的需求
        
考虑到这些不同，以及大多数应用程序在调用网络层的时候通常会期望一个阻塞模式，最好的解决方案就是通过写一个阻塞模式的模仿框架来隐藏掉这一表象。这就是 MINA 所做的事情！
        
但是 MINA 做的事情不仅于此。它为需要通过 TCP、UDP 或者任何机制通信的应用提供了一个通用 IO 视觉。如果我们仅仅考虑 TCP 或者 UDP，一个是有连接的协议 (TCP) 另一个是无连接的 (UDP)。MINA 将这种差异掩盖起来，以让你关注于对你的应用很重要的两部分：实用的代码和应用协议的编码/解码。
        
MINA 不仅仅处理 TCP 和 UDP，它也使用 VmpPipe 或者 APR 提供了一个串行通信 (RSC232) 之上的一层。
        
最后，很重要的是，MINA 是一个专门设计既能工作在客户端又能工作在服务器端的网络框架。写一个服务器的关键在于具有一个可扩展性的系统，这样可以灵活地满足服务器需求，根据性能和内存使用：这就是 MINA 的优势，使你的服务器开发变得容易。

##何时使用 MINA？

这是个有趣的话题！MINA 并不期望在任何情况下都是最好的选择。在考虑使用 MINA 之前要思考几个要素：

* 易于使用。在你没有特别性能要求时，MINA 会是一个好的选择，因为它可以让你轻松地开发一个服务器或者客户端，在 BIO 或者 NIO 之上写同样的应用时不需要应付各种参数和用例。只需要几十行代码就可以写你的服务器，减少一些你可能会落入的陷阱。
* 高并发的用户量。 BIO 明显地要比 NIO 快。这点使得 30% 的用户喜欢 BIO。对于数千个用户连接的情况确实如此，但是一旦达到某个点，BIO 方式将会停止扩展，你无法使用一个线程一个用户的方式来处理上百万用户的并发。NIO 可以。现在，另一方面是，相对于你的应用所消耗的时间，你的代码中花在 MINA 部分的时间实际可能是微不足道的。在某种情况下，不需要花费更多时间和精力来写一个更快的网络层以获得些许提升的目标。
* 被证明的系统。MINA 已被全球数以万计的应用所使用。也有一些基于 MINA 的 Apache 项目，而且它们工作的相当好。这就是某种形式的担保，你不需要为你网络传输层的实现的一些神秘的错误而花费大量的时间。
* 现有协议的支持。MINA 附带有对各种现有协议的实现：HTTP、XML、TCP、LDAP、DHCP、NTP、DNS、XMPP、SSH、FTP ...在某种情况下，MINA 不仅可以作为一个 NIO 框架，也可以作为一个具有各种协议实现的网络传输层。MINA 近期要推出的新特性之一就是提供一个你可以使用现有的协议的集合。