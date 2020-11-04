## BIO和NIO和AIO
* **NIO**
* BIO 采用的是一种多路复用的机制，利用单线程轮询事件，高效定位就绪的 Channel 来决定做什么，只是 Select 阶段是阻塞式的，能有效避免大量连接数 时，频繁线程的切换带来的性能或各种问题。 先，Requester方通过Selector.open()创建了一个Selector准备好了调度角色。 创建了 SocketChannel(ServerSocketChannel) 并注册到 Selector 中，通过设置 key（SelectionKey）告诉调度者所应该关注的连接请求。 阻塞，Selector 阻塞在 select 操作中，如果发现有 Channel 发生连接请求，就会唤醒处理请求。
* NIO 对比 BIO 的同步阻塞 IO 操作，实际上 NIO 是同步非阻塞 IO，一个线程在同 步的进行轮询检查，Selector 不断轮询注册在其上的 Channel，某个 Channel 上面发生读写连接请求，这个 Channel 就处于就绪状态，被 Selector 轮询出来， 然后通过 SelectionKey 可以获取就绪 Channel 的集合，进行后续的 I/O 操作。 同步和异步说的是消息的通知机制，这个线程仍然要定时的读取 stream，判断数据有没有准备好，client 采用循环的方式去读取（线程自己去 抓去信息），CPU 被浪费。 非阻塞：体现在，这个线程可以去干别的，不需要一直在这等着。Selector 可以同时轮询多个 Channel，因为 JDK 使用了 epoll()代替传统的 select 实现，没有最大连接句柄限制。所以只需要一个线程负责 Selector 的轮 询，就可以接入成千上万的客户端
* AIO 不需要通过多路复用器对注册的通道进行轮询操作即可实现异 步读写。什么意思呢？NIO 采用轮询的方式，一直在轮询的询问 stream 中数据 是否准备就绪，如果准备就绪发起处理。但是AIO就不需要了，AIO框架在windows 下使用 windows IOCP 技术，在 Linux 下使用 epoll 多路复用 IO 技术模拟异步 IO， 即：应用程序向操作系统注册 IO 监听，然后继续做自己的事情。操作系统 发生 IO 事件，并且准备好数据后，在主动通知应用程序，触发相应的函数（这 就是一种以订阅者模式进行的改造）。由于应用程序不是“轮询”方式而是订阅 -通知方式，所以不再需要 selector 轮询，由 channel 通道直接到操作系统注册 监听。
* **缓冲区，通道，多路复用器**
* 缓冲区 Buffer NIO 基于块进行数据处理，在 NIO 中所有数据的读取都是通过缓冲 Buffer 进行 处理。 具体的缓存区有这些：ByteBuffe、CharBuffer、 ShortBuffer、 IntBuffer、LongBuffer、FloatBuffer、DoubleBuffer。他们实现了相同的接口： Buffer。 
* 通道 Channel 对数据的读取和写入要通过 Channel 通道。通道不同于流的地方就是通 道是双向的，用于读、写和同时读写操作。底层的操作系统的通道一般都是全双 工的，全双工的 Channel 比流能更好的映射底层操作系统的 API。
* 多路复用器 Selector Selector 提供选择已经就绪的任务的能力： Selector 轮询注册在其上的 Channel，如果某个 Channel 发生读写请 求 并 且 Channel 就 处 于 就 绪 状 态 ， 会 被 Selector 轮 询 出 来 ， 然 后 通 过 SelectionKey 可以获取就绪 Channel 的集合，进行后续的 I/O 操作。（同步） 一个 Selector 可以同时轮询多个 Channel，因为 JDK 使用了 epoll() 代替传统的 select 实现，所以没有最大连接句柄 1024/2048 的限制。所以，只 需要一个线程负责 Selector 的轮询，就可以接入成千上万的客户端。（非阻塞）