## 进程间通信

**进程间通信有哪些方式**

* 管道（pipe）：管道是一种半双工的通信方式，数据只能单向流动，而且只能在有血缘关 系的进程间使用，进程的血缘关系通常是指父子进程关系。
* 命名管道（named pipe）：也是半双工的通信方式，但是它允许无亲缘关系关系进程间通信。
* 信号（signal）：是一种比较复杂的通信方式，用于通知接收进程某一事件已经发生。
* 信号量（semophere）：信号量是一个计数器，可用来控制多个进程对共享资源的访问。 它通常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此， 主要作为进程间以及同一进程内不同线程之间的同步手段。
* 消息队列（message queue）:消息队列是由消息组成的链表，存放在内核中，并由消息队 列标识符标识。消息队列克服了信号传递消息少，管道只能承载无格式字节流以及缓冲区大 小受限等缺点。
* 共享内存（shared memory）:就是映射一段能被其他进程所访问的内存，这段共享内存由 一个进程创建，但多个进程都可以访问，共享内存是最快的 IPC 方式，它是针对其他进程间 的通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号量等配合使用，来实 现进程间的同步和通信。
* 套接字（socket）：套接口也是进程间的通信机制，与其他通信机制不同的是它可用于不 同及其间的进程通信。
 
**区别**

* 套接字（socket）：套接口也是进程间的通信机制，与其他通信机制不同的是它可用于不 同及其间的进程通信。
* 管道：速度慢、容量有限
* 消息队列：容量收到系统限制，且要注意第一次读的时候，要考虑上一次没有读完数据的问题。
* 信号量：不能传递复杂信息，只能用来同步。
* 共享内存：能够很容易控制容量，速度快，但要保持同步，比如一个进程在写的时候，另一个进程要注意读写的问题，相当于线程中的线程安全。