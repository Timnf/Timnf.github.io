---
title: C++常见知识掌握
date: 2024-05-20
tags: CPP
categories: 编程语言
---

1. Linux软件开发、调试与维护
内核与系统结构
Linux内核是操作系统的核心，负责管理硬件资源，提供系统服务，它是系统软件与硬件之间的桥梁。主要组成部分包括：
进程管理：内核通过调度器分配CPU时间给各个进程，实现进程的创建、调度、终止等操作。使用进程描述符（task_struct）来存储进程信息，包括状态（就绪、运行、阻塞等）、优先级、内存映射等。
内存管理：包括物理内存和虚拟内存管理。通过页表映射实现虚拟地址到物理地址的转换，使用伙伴系统算法管理空闲内存块，还有分页、交换、内存缓存等机制来优化内存使用。
文件系统：内核提供了VFS（虚拟文件系统）层，使得不同的文件系统可以统一接口访问。支持多种文件系统，如EXT4、XFS等。
网络堆栈：实现了TCP/IP协议栈，负责网络数据包的发送、接收、路由和错误处理。包括网络设备驱动、IP层、传输层（TCP、UDP）和应用层接口。
<!--more-->
文件系统管理
EXT4：是Linux系统中广泛使用的日志文件系统，支持大文件（最大16TB）、大卷（最大1EB）、快速文件系统检查、延迟分配等特性。适合大数据量、高吞吐量的应用。
XFS：最初为SGI的IRIX系统设计，后被移植到Linux，以其高性能和稳定性著称。支持非常大的文件和文件系统大小（最大文件和文件系统大小均为16EB），高效的并行I/O处理，适合大型数据库和文件服务器。
挂载与权限：文件系统需挂载到目录树的某个点才能被访问，使用mount命令完成。权限管理包括读、写、执行权限（rwx）以及所有者、所属组、其他人的权限设置。使用chmod, chown, chgrp命令调整权限和所有权。此外，sudo命令允许特定用户以超级用户或其他用户身份执行命令。
SELinux/AppArmor：这两种都是强制访问控制（MAC）机制，增强了系统的安全性。SELinux基于类型强制访问控制（TE），通过标签定义主体和客体的安全上下文，严格限制进程对资源的访问。AppArmor则基于路径和规则集，限制进程可以访问的文件和资源，相对简单易用。

进程间通信（IPC）
Linux中进程间通信的方式多样，包括但不限于：
管道（pipe）和命名管道（FIFO）：用于父进程和子进程或无关进程间的单向通信，命名管道可以在文件系统中创建，供任意进程使用。
信号（signal）：一种进程间传递消息的方式，可以通知进程某个事件的发生，如终止进程、暂停等。
消息队列：提供了一种在不同进程间传递数据报文的机制，支持消息的排队和同步访问。
共享内存：最直接的通信方式，两个或多个进程可以直接访问同一块内存区域，速度快但需要处理同步问题。
信号量：用于解决进程间的同步问题，可以控制多个进程对共享资源的访问顺序和数量。
套接字（socket）：不仅可用于网络通信，也支持进程间通信，支持多种通信模式，如TCP（可靠连接）、UDP（无连接）等。

线程间通信
线程作为进程内的执行单元，通信方式相比进程间通信更为灵活且开销更小，主要方式包括：
共享变量：线程共享进程的地址空间，可以直接访问同一变量，但需注意同步问题，如使用互斥锁（mutex）、原子操作等。
线程局部存储：每个线程可以有自己的独立存储空间，避免全局变量的竞争问题。
条件变量：配合互斥锁使用，用于线程间的同步，一个线程等待条件满足，另一个线程改变条件后唤醒等待的线程。
信号量：同进程间通信中的信号量，也可用于线程间同步。
无锁编程技术：如CAS（Compare and Swap）等原子操作，避免使用锁带来的性能损耗，适合高并发场景。

C/C++语言与Linux编程
智能指针 (Smart Pointers)
std::unique_ptr：独占所有权的智能指针，保证同一时间内只有一个智能指针指向资源，离开作用域时自动释放资源。
std::shared_ptr：共享所有权的智能指针，允许多个智能指针指向同一资源，资源的生命周期取决于引用计数，当最后一个指向它的智能指针销毁时，资源被释放。
std::weak_ptr：弱引用智能指针，用来观察但不增加被观测的shared_ptr的引用计数，避免循环引用导致的内存泄漏。
Lambda表达式
Lambda表达式允许在代码中直接定义匿名函数对象，常用于回调函数、算法的自定义行为等场景。例如：

```Cpp
auto add = [](int x, int y) { return x + y; };
cout << add(3, 4); // 输出7
```
多线程支持
C++11引入了 <thread> 库，支持多线程编程。可以创建、启动、等待线程，以及进行线程间的同步：

```Cpp
#include <iostream>
#include <thread>

void threadFunction() {
    std::cout << "Hello, World from thread!" << std::endl;
}

int main() {
    std::thread myThread(threadFunction);
    myThread.join(); // 等待线程结束
    return 0;
}
```

STL：举例说明STL容器（vector,list,queue, map）和算法（sort, find）的使用。
std::vector（向量）
vector 是一个动态数组，其特点是元素在内存中连续存储。它内部维护了三个重要成员：

start：指向数组起始位置的指针。
finish：指向数组最后一个元素之后的位置的指针。
end_of_storage：表示分配给vector的内存空间的结束位置。
当向vector添加元素超过当前容量时，它会重新分配更大的内存块（通常是当前容量的两倍），然后将原数据复制到新位置，并释放旧内存。这个过程称为重新分配，可能会导致性能上的开销。

std::list（双向链表）
list 实现为双向链表，每个节点包含数据和两个指针，分别指向前一个节点和后一个节点。因此，list中的元素不需要连续存储，插入和删除操作非常高效，时间复杂度为O(1)，但随机访问元素较慢，需要从头或尾开始遍历。

std::queue（队列）
queue 是一种适配器容器，它基于deque（双端队列，默认情况下）或其他容器（如list）实现。它遵循先进先出（FIFO）原则，只允许在一端（后端）添加元素，在另一端（前端）移除元素。由于是基于deque实现，所以两端操作都是高效的。

std::map（映射）
map 是一种关联容器，它以键值对的形式存储元素，键是唯一的。内部通常采用红黑树实现，这确保了对数时间复杂度的查找、插入和删除操作。红黑树是一种自平衡二叉搜索树，通过颜色标记和旋转操作维持树的平衡，从而保证了良好的性能。

键唯一性：通过比较键的顺序来维持。
迭代器：提供了从左到右按升序访问元素的迭代器。
插入和删除：保持树的平衡和键的有序性，最坏情况下的时间复杂度为O(log n)。
每种容器的选择依赖于具体的应用需求，如是否需要快速随机访问（vector）、频繁的插入删除（list）、特定的访问模式（queue）或是键值对的存储与查找（map）。

std::sort：通用排序算法，对区间内的元素进行排序，需要区间支持随机访问迭代器。
```Cpp
std::vector<int> vec = {3, 1, 4, 1, 5, 9, 2, 6};
std::sort(vec.begin(), vec.end());
```
std::find：在区间内查找指定值的第一个出现位置，返回迭代器。
```Cpp
if(std::find(vec.begin(), vec.end(), 5) != vec.end()) {
    std::cout << "Found!" << std::endl;
}
```

网络编程
TCP/IP协议栈是互联网通信的基础，其中TCP（Transmission Control Protocol，传输控制协议）和UDP（User Datagram Protocol，用户数据报协议）是两个重要的传输层协议。下面分别概述TCP的三次握手和四次挥手过程，以及UDP的主要特点。

TCP三次握手
TCP三次握手是TCP连接建立的过程，确保了两方准备好进行可靠的数据传输。握手步骤如下：
同步请求（SYN）：客户端发送一个带有SYN标志的TCP段给服务器，并包含一个初始序列号(ISN, Initial Sequence Number)。这表示客户端想要建立连接。
同步确认与确认（SYN-ACK）：服务器接收到SYN后，回应一个带有SYN和ACK标志的TCP段。这个段中包含服务器的初始序列号和对客户端ISN+1的确认（即ACK）值。这表明服务器同意连接，并告知客户端其序列号。
确认（ACK）：客户端收到SYN-ACK后，发送一个只带有ACK标志的TCP段给服务器，确认序列号设置为服务器的ISN+1。这表示客户端已收到服务器的SYN且连接已建立。

TCP四次挥手
TCP四次挥手是TCP连接断开的过程，确保了双方都同意关闭连接，并且数据传输已经完成。
终止请求（FIN）：当一方完成数据发送后，会发送一个带有FIN标志的TCP段给对方，表示没有更多数据要发送了。
确认（ACK）：接收方收到FIN后，回送一个ACK，确认接收到FIN请求，但此时连接还未完全关闭，因为可能还有数据需要从接收方传回给发送方。
终止请求（FIN）：当接收方也完成数据发送后，也会发送一个FIN给对方。
最终确认（ACK）：最初发送FIN的一方收到FIN后，发送最后一个ACK，确认连接可以关闭。之后等待一段时间（TIME_WAIT状态），确保最后一个ACK能够到达，然后关闭连接。

UDP特点
UDP是一种无连接的、不可靠的传输协议，相对于TCP，它有以下特点：
无连接性：UDP不建立连接，发送数据前不需要预先建立连接，因此减少了开销和延迟。
尽最大努力交付：UDP不保证数据包的顺序、也不保证每个数据包都能成功到达目的地，它是不可靠的。
头部开销小：UDP头部只有8个字节，相比TCP头部简单，适用于需要高效传输小数据包的场景。
无拥塞控制：UDP没有像TCP那样的流量控制和拥塞控制机制，适合于实时应用如音视频流、在线游戏等，这些应用即使丢包也不希望有太大的延迟。
支持多播和广播：UDP协议天然支持多播和广播，使得一个数据包能同时发送给多个接收者，这对于某些类型的网络服务非常有用。


Socket编程：解释服务器端和客户端套接字编程流程，同步/异步I/O模型。
服务器端套接字编程流程
初始化套接字库：
首先，服务器端需要通过调用如socket()函数来初始化套接字库，该函数会返回一个新的套接字描述符，它是后续操作的句柄。
创建套接字：
使用socket()函数创建一个套接字，指定地址族（如AF_INET用于IPv4）、套接字类型（SOCK_STREAM用于TCP，SOCK_DGRAM用于UDP）和协议（通常为0，让系统自动选择）。
绑定IP和端口：
通过bind()函数将创建的套接字与一个IP地址和端口号绑定，这样客户端就可以通过这个地址和端口找到服务器。
监听连接：
调用listen()函数使套接字处于监听状态，准备接受客户端的连接请求。需要指定最大连接队列长度。
接受连接：
使用accept()函数阻塞等待，直到有客户端连接请求到达。成功连接后，会返回一个新的套接字描述符，这个描述符专门用于与这个客户端通信，原监听套接字继续监听新的连接请求。
数据收发：
通过recv()或send()函数与客户端进行数据交换。recv()用于接收客户端发来的数据，send()用于向客户端发送数据。
关闭连接：
当数据传输完毕，通过close()函数关闭与客户端通信的套接字描述符。若不再接受新连接，还需关闭监听套接字。

客户端套接字编程流程
初始化套接字库：
同服务器端，首先调用socket()函数初始化套接字库。
创建套接字：
使用socket()函数创建套接字，指定地址族和套接字类型。
连接服务器：
通过connect()函数，使用服务器的IP地址和端口号建立连接。这一步会阻塞直到连接建立或失败。
数据收发：
连接成功后，客户端可以通过send()发送数据给服务器，通过recv()接收来自服务器的数据。
关闭连接：
数据交换完毕后，调用close()关闭套接字，结束通信。

同步/异步I/O模型
同步I/O：
在同步I/O模型中，调用者必须等待I/O操作完成才能继续执行。例如，在上述流程中，accept()、recv()、send()等都是阻塞调用，意味着调用它们的线程会暂停，直到操作完成或发生错误。这种方式编程简单，但可能造成线程阻塞，影响并发处理能力。
异步I/O：
异步I/O允许调用者立即返回，继续执行其他任务，而I/O操作由系统在后台完成，完成后通过回调函数、事件或信号通知调用者。在一些高级语言或框架中，通过非阻塞套接字配合多路复用（如epoll、kqueue）或异步I/O库（如libuv、Boost.Asio）实现。异步模型能更高效地处理大量并发连接，提高系统吞吐量，但编程模型相对复杂。
扩展
多线程/多进程模型：在同步I/O基础上，可以通过多线程或多进程来同时处理多个连接，提高服务器的并发处理能力，但这会增加资源消耗和管理复杂度。
事件驱动模型：在异步I/O基础上，使用事件循环和回调函数来响应I/O事件，如Node.js的EventEmitter、Python的asyncio，这种模型特别适合处理大量并发连接，减少系统开销。
协程：现代编程语言中（如Python的async/await、Go的goroutines），协程提供了一种轻量级的并发模型，结合异步I/O，可以在单线程内实现高性能的并发处理，代码风格更加直观易读。

select, poll, epoll, 和 ST 库中的 st_poll 都是用于实现I/O多路复用的技术，允许单个进程或线程高效地监视多个文件描述符（如套接字），等待它们变为可读、可写或出现异常等事件。下面是对这四者的比较和区别：
1. Select
特点:
基础的I/O多路复用方法，历史悠久，几乎在所有类Unix系统上可用。
需要将所有待检测的文件描述符从用户空间复制到内核空间，开销较大，特别是当文件描述符数量较多时。
支持的文件描述符数量有限，通常默认为1024。
时间复杂度为O(n)，因为每次调用都需要遍历整个文件描述符集合。
可以设置超时时间，精度较高（微秒级别）。
2. Poll
特点:
相比select，poll解决了文件描述符数量限制的问题，因为它使用链表来存储文件描述符，理论上支持的描述符数量更大。
同样需要在每次调用时将文件描述符集合从用户空间复制到内核空间。
时间复杂度也是O(n)，因为它同样需要遍历整个描述符集合来查找就绪的文件描述符。
提供了与select相似的功能，但更灵活。
3. Epoll
特点:
Linux特有的I/O多路复用技术，性能优于select和poll。
采用事件驱动模型，只需一次调用epoll_ctl注册感兴趣的事件，后续调用epoll_wait时，内核只返回实际就绪的文件描述符，时间复杂度接近O(1)。
支持水平触发（LT）和边缘触发（ET）两种工作模式，后者在高并发场景下更为高效。
减少了用户空间和内核空间的频繁数据复制，减少了系统调用次数，适合大量并发连接的场景。
4. ST 中的 Poll (st_poll)
特点:
这是ST库提供的一个自定义的poll实现，旨在与ST的线程模型和其他特性（如中断处理、线程上下文切换）无缝集成。
包含了中断处理逻辑，允许线程在等待I/O时可以被其他线程中断，提高了线程的响应性和灵活性。
可能包含了ST库特有的调度和线程管理机制，如加入和移出I/O等待队列、睡眠队列等。
与标准poll相比，它可能提供了更高级别的抽象，更易于在ST环境下编写多线程、可中断的I/O密集型应用。
总结
性能: epoll > poll > select，st_poll在ST环境中提供额外的线程管理和中断处理能力，但其基本I/O检测效率与原生poll相近。
可移植性: select和poll较为通用，而epoll仅限于Linux。
功能与集成: st_poll在ST线程库的上下文中提供了更紧密的集成和高级特性，比如线程中断处理，更适合于使用ST库开发的应用程序。

Windows 和 macOS（以前称为 Mac OS X）操作系统中，用于实现多路复用的I/O技术有所不同，主要是因为它们基于不同的内核和系统调用接口。以下是两者中常见的多路复用技术：
Windows:
I/O完成端口 (I/O Completion Ports, IOCP):
IOCP 是Windows下高效处理大量并发I/O请求的机制，尤其适用于高吞吐量的网络服务和磁盘I/O。它允许应用程序异步地提交I/O操作，并在操作完成时接收通知。IOCP可以看作是Windows平台上的高性能多路复用技术。
Select:
尽管不如IOCP高效，但Windows也支持传统的select系统调用，用于监视多个文件描述符上的I/O事件，尽管它在处理大量文件描述符时的性能不佳。
WaitForMultipleObjects:
这个API可以用来等待一组对象（包括线程、事件、互斥锁等）中的任何一个或全部进入已通知状态。虽然它不仅仅用于I/O多路复用，但在某些场景下也可以用来实现类似的功能。

macOS:
kqueue:
macOS 和其他类BSD系统中广泛使用的多路复用技术，提供了比传统select和poll更高效和灵活的事件通知机制。kqueue允许监听文件描述符上的读、写、关闭、挂载等各种事件，并且支持过滤器来细化监控条件。
poll:
macOS 也支持poll系统调用，它可以监视多个文件描述符的读、写事件，相比select在处理大量文件描述符时有所改进。
select:
作为最基础的多路复用技术，select在macOS上也是可用的，尽管它的性能和可扩展性较差。
综上所述，虽然两系统都支持经典的select和poll，Windows倾向于使用更高效的IOCP，而macOS推荐使用更现代且功能丰富的kqueue。开发者在选择多路复用技术时应考虑具体的应用需求、性能要求以及操作系统的特性。

HTTP/SSL/TLS：简述HTTP请求响应流程，SSL/TLS加密握手过程，以及如何使用OpenSSL库。
HTTP 请求响应流程
客户端发起请求：客户端（如浏览器）构建一个HTTP请求报文，包括请求行（方法、URL、协议版本）、请求头（携带客户端信息、请求内容类型等）和可能的请求正文（POST请求时）。常见的请求方法有GET、POST等。
建立TCP连接：客户端使用TCP的三次握手与服务器建立连接，为HTTP通信提供可靠的传输通道。
发送请求：建立连接后，客户端通过这个TCP连接发送HTTP请求报文到服务器。
服务器处理请求：服务器接收到请求后，解析请求报文，根据请求的内容执行相应操作，比如查询数据库、执行脚本等。
服务器响应：服务器构建响应报文，包括状态行（协议版本、状态码、状态消息）、响应头（服务器信息、内容类型等）和响应正文（如HTML页面、图片数据等）。然后通过TCP连接将响应报文发送给客户端。
关闭连接或保持连接：根据HTTP协议版本和请求头中的Connection字段，决定是否关闭TCP连接。HTTP 1.0默认每次请求后关闭连接，而HTTP 1.1引入了Keep-Alive机制，可以复用连接发送多个请求。

SSL/TLS 加密握手过程
SSL/TLS握手是一个协商加密参数的过程，分为几个阶段：
Hello：客户端发送ClientHello消息，包含支持的协议版本、加密套件列表、随机数等信息。服务器回应ServerHello，确认协议版本、选择的加密套件、服务器随机数等。
证书交换：服务器发送其数字证书，证明自己的身份。证书包含公钥，客户端验证证书的有效性（包括验证证书链到信任的CA、检查证书有效期等）。
密钥交换与认证：客户端生成预主密钥（Pre-Master Secret），使用服务器公钥加密后发送给服务器。双方使用预主密钥、随机数等信息计算出会话密钥（Session Key），用于对称加密后续通信。
握手完成：双方通过Change Cipher Spec消息通知对方开始使用新密钥和算法进行加密通信。随后发送Finished消息，包含之前所有握手消息的哈希值，验证握手过程中没有被篡改，并切换到加密通信。

使用 OpenSSL 库
OpenSSL 是一个强大的安全套接字层密码库，支持SSL/TLS协议，常用于实现加密通信。
安装与配置
下载并安装OpenSSL库。
在开发环境中配置好头文件路径和库文件路径。
基本使用示例
生成密钥和证书：使用OpenSSL命令行工具生成私钥和自签名证书，或申请CA签发的证书。
SSL上下文初始化：使用SSL_CTX_new()函数创建一个SSL上下文，它是SSL会话的配置中心，包括协议版本、加密套件等。
加载证书和私钥：通过SSL_CTX_use_certificate_file()和SSL_CTX_use_PrivateKey_file()加载证书和私钥到SSL上下文中。
创建SSL对象：每建立一个TCP连接，就使用SSL_new()创建一个SSL对象，与该连接关联。
SSL连接建立：在TCP连接的基础上，使用SSL_accept()（服务器端）或SSL_connect()（客户端）进行SSL/TLS握手。
数据加密传输：通过SSL_read()和SSL_write()进行加密的数据读写操作。
清理：通信结束后，使用SSL_free()释放SSL对象，SSL_CTX_free()释放SSL上下文。
OpenSSL提供了丰富的API，支持复杂的SSL/TLS特性，如会话缓存、ALPN协议协商等，可以根据具体需求定制化实现。

跨平台开发有哪些需要处理的工作。
针对一些处理器存在C++支持不是很好的问题。可能考虑需要使用c来实现。
针对一些系统接口，如IO操作和网络通信，还有一些特定的系统模块操作，如我们的android 下 oma通道和sim卡通信，在模组rtos上需要使用模组自带的API接口来做通信。字节序和数据类型的大小：不同的操作系统字节序不同，还有就是数据类型的大小也不一样。文件路径和分隔符不同，线程和并发模型不同。POSIX线程与windows线程。
针对这些问题，可以使用一个统一API层来封装底层的不用API，已实现上层操作统一。一般会使用预处理器宏定义来区分特定的代码。
编译器问题，需要使用不同的编译器或工具链。可以使用CMake的，建议使用。其优点很好。具体优点。。。

公私钥体系可以做的事？
**加密与解密：**
加密：发送方使用接收方的公钥对信息进行加密，确保只有拥有对应私钥的接收方能够解密信息。这保证了信息的机密性，因为没有私钥，即使是截获了加密信息的第三方也无法解密。
解密：接收方用自己的私钥对加密信息进行解密，恢复原始信息。由于公钥是公开的，任何人都可以加密信息，但只有私钥持有者能解密，确保了安全通信。
**数字签名：**
签名：发送方使用自己的私钥对信息的摘要（通常是经过哈希运算后的固定长度数据）进行加密，生成签名。这相当于确认信息出自发送方并且未被篡改。
验证：接收方使用发送方的公钥验证签名，确保信息的完整性和来源的真实性。如果信息被修改，签名将无法通过验证。
**身份验证：**
个人或系统可以通过展示能够使用对应私钥进行某种操作（如解密或签名）的能力来证明其身份。这在无密码登录、安全认证等领域广泛应用。
**密钥交换：**
在对称加密中，需要安全地交换密钥。公私钥体系可以用来安全地传递对称密钥，即用接收方的公钥加密对称密钥，然后安全地传输，只有接收方可以用其私钥解密。
**访问控制：**
公私钥对可以作为访问控制机制的一部分，例如，只允许持有特定私钥的用户或系统访问受保护的资源。
软件签名与验证：
开发者可以用私钥对软件或代码签名，用户使用开发者公钥验证签名，确保软件未被篡改且来自可信源。
**Web安全：**
HTTPS协议利用SSL/TLS协议，其中涉及到公钥加密来保护Web通信，确保数据在传输过程中的安全。

工作中遇到的影响深刻的问题，怎么解决的？
在回答关于工作中遇到的深刻问题及其解决方案时，一个结构清晰且有说服力的回答通常包括以下几个部分：
针对C/C++工程师，下面是一个资深开发者解决内存泄漏问题的例子：

问题背景：在开发一个高性能服务器应用程序的过程中，我们的团队遇到了严重的性能瓶颈问题。该程序用C++编写，负责处理高并发的网络请求。随着运行时间的增长，服务器的响应速度逐渐下降，直至服务几乎不可用。

问题描述：经过初步调查，我们发现内存使用量随时间线性增长，表明存在内存泄漏。这是一个严重的问题，因为长时间运行的服务无法承受持续的内存消耗。

分析过程：

初步诊断：使用Valgrind和AddressSanitizer工具进行内存泄漏检测，发现有几个可疑的内存分配未被释放。
深入探究：通过代码审查和动态调试，我们追踪到内存泄漏的源头在于一个复杂的网络事件回调逻辑中，特定条件下的事件处理函数忘记释放分配的临时对象。
解决方案：

方案提出：为了解决这个问题，我们决定引入智能指针（std::unique_ptr和std::shared_ptr），自动管理对象生命周期，减少手动管理内存的负担。
实施步骤：重构相关代码，用智能指针替换裸指针，确保每个对象在不再使用时能够被正确释放。同时，加强单元测试，特别是针对边界条件和异常处理的测试。
效果评估：实施后，内存泄漏问题得到彻底解决，服务器在高负载下连续运行数天，内存使用保持稳定，性能显著提升，平均响应时间缩短了30%。
经验总结与反思：

学到的经验：这次经历让我们深刻认识到自动化内存管理工具和现代C++特性在避免内存管理错误中的重要性。同时，我们也意识到定期进行性能监控和压力测试对于提前发现此类问题是多么关键。
后续改进：为了防止未来再次发生类似问题，我们决定在CI/CD流程中集成内存泄漏检测工具，并定期进行代码审查，特别关注内存管理实践。此外，团队内部开展了一轮关于现代C++最佳实践的培训，以提升整体开发水平。


不要总是强调开源什么的。显的没有核心技术。
团队管理经验可以写下。

1. 产品设计与开发
软件设计模式
单例模式：应用场景，实现方式。
工厂模式：优点，如何解耦。
观察者模式：如何实现事件驱动编程。
数据结构与算法
链表、树、图：每种数据结构的应用场景，基本操作（如搜索、插入、删除）。
排序与查找算法：快速排序、二分查找等算法原理，复杂度分析。
1. 测试支持与问题定位
单元测试与集成测试
Google Test：介绍如何编写测试用例，断言的使用。
gdb调试：如何设置断点、查看变量值、单步执行等基本调试技巧。
问题分析技巧
strace：如何跟踪系统调用，诊断文件访问、网络通信等问题。
ltrace：跟踪库函数调用，适用于C库函数分析。
netstat：查看网络连接状态，端口监听情况。
1. 系统部署与客户支持
FAE与售后支持
环境搭建：描述自动化部署工具（Ansible, Docker）的使用。
版本控制：Git工作流（如GitFlow），解决冲突的策略。
需求分析与定制
需求收集：与客户交流的技巧，需求分析文档编写。
定制化方案：根据客户需求调整产品功能，平衡技术可行性与客户需求。
1. 附加技能点
Qt / Electron 开发
Qt简介：C++界面开发，QML语言，信号槽机制。
Electron：基于Node.js和Chromium的跨平台桌面应用开发，渲染进程与主进程的概念。
嵌入式软件开发
嵌入式Linux：裁剪内核，交叉编译环境搭建。
ARM架构：与x86架构的对比，ARM处理器系列。
RTOS：实时操作系统的概念，与Linux的区别。
