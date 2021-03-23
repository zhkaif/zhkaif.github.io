---
title: BIO/NIO/AIO对比
date: 2021-03-22 10:14:29
tags:
  - IO模型
categories: [后端, Java]
---

## IO 模型

就是使用什么样的通道进行数据的发送和接收，很大程度上决定了程序通信的性能。
Java 支持三种网络编程模型：BIO、NIO、AIO。

- Java BIO，同步并阻塞（传统阻塞型），服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，可以通过线程池机制改善。

- Java NIO，同步非阻塞，服务器实现模式为一个线程处理多个请求（连接），即客户端发送的连接请求会被注册到多路复用器上，多路复用器轮询到有 IO 请求就会进行处理。

- Java AIO，异步非阻塞，AIO 引入了异步通道的概念，采用了 Proactor 模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用。

## BIO、NIO、AIO 使用场景

- BIO 方式适用于连接数比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4 之前唯一的选择，程序较为简单容易理解。
- NIO 方式适用于连接数目多且连接比较短的架构，比如聊天服务器，弹幕系统，服务器间通讯等，编程比较复杂，JDK1.4 开始支持。
- AIO 方式适用于连接数目多且连接比较长的架构，比如相册服务器，充分调用 OS 参与并发操作，编程比较复杂，JDK7 开始支持。

## BIO 基本介绍

- Java BIO 就是传统的 Java IO 编程，其相关的类和接口在 java.io 包下。
- BIO（Blocking I/O）：同步阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时，服务器就会需要启动一个线程来进行处理。如果这个连接不做任何事情就会造成不必要的线程开销，可以通过线程池机制改善。

### BIO 编程简要流程

1. 服务器驱动一个 ServiceSocket。
2. 客户端启动 Socket 对服务器进行通信，默认情况下服务器端需要对每一个客户端建立一个线程进行通信。
3. 客户端发出请求后，先咨询服务器是否线程响应，如果没有则会等待，或者被拒绝。
4. 如果有响应，客户端线程会等待请求结束后，再继续执行。

### BIO 服务端代码案例

```Java
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * @author zhkai
 * @date 2021年3月22日14:35:59
 */
public class Bio {


    /**
     * BIO 测试方法
     */
    public static void bioTest() throws IOException{
        // 创建线程池
        ExecutorService executorService = Executors.newCachedThreadPool();
        // 创建serverSocket
        ServerSocket serverSocket = new ServerSocket(6666);
        for (; ; ) {
            System.out.println("等待连接中...");
            // 监听，等待客户端连接
            Socket socket = serverSocket.accept();
            System.out.println("连接到一个客户端");
            executorService.execute(() -> handler(socket));
        }
    }

    /**
     * 编写一个handler方法，和客户端通讯
     *
     * @param socket 套接字
     */
    public static void handler(Socket socket) {
        byte[] bytes = new byte[1024];
        System.out.println("当前线程信息：" + Thread.currentThread().getName());
        try {
            // 通过socket获取输入流
            InputStream inputStream = socket.getInputStream();
            // 循环读取客户度发送的数据
            while (inputStream.read(bytes) != -1) {
                System.out.println(Thread.currentThread().getName() + "：发送信息为：" + new String(bytes, 0, bytes.length));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            System.out.println("关闭连接");
            try {
                socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

- 启动该案例
- 使用终端命令`telnet 127.0.0.1 6666`，打开 telnet 命令窗口
  ![avatar](https://img.imgdb.cn/item/60582d968322e6675c66aa1d.jpg)
- 按下`ctrl+]`
  ![avatar](https://img.imgdb.cn/item/60582da28322e6675c66b0b1.jpg)
- 输入`send hello world`，进行通信
  ![avatar](https://img.imgdb.cn/item/60582da98322e6675c66b3ca.jpg)
- 输入`quit`退出 telnet 窗口，关闭连接
  ![avatar](https://img.imgdb.cn/item/60582daf8322e6675c66b674.jpg)

### BIO 问题分析

1. 每个请求都需要创建独立的线程，与对应的客户端进行数据处理。
2. 当并发数大时，需要创建大量线程来处理连接，系统资源占用大。
3. 连接建立后，如果当前线程暂时没有数据可读，则当前线程会一直阻塞在 Read 操作上，造成线程资源浪费。

## NIO 基本介绍

1. Java NIO 全称 Java non-blocking IO，指的是 JDK 提供新的 API。从 JDK1.4 开始，Java 提供了一系列改进的输入/输出的新特性，被统称为 NIO，即 New IO，是同步非阻塞的。
2. NIO 相关类都放在 java.io 包下，并对原 java.io 包中很多类进行了改写。
3. NIO 有三大核心部分：Channel（管道）、Buffer（缓冲区）、Selector（选择器）。
4. NIO 是面向缓冲区编程的，数据读取到一个它稍微处理的缓冲区，需要时可在缓冲区中前后移动，这就增加了处理过程的灵活性，使用它可以提供非阻塞的高伸缩性网格。
5. Java NIO 的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用数据，如果目前没有可用数据时，则说明都不会获取，而不是保持线程阻塞，所以直到数据变为可以读取之前，该线程可以做其他事情。非阻塞写入同理。

### NIO Buffer 的基本使用

```Java
  import java.nio.IntBuffer;

/**
 * @author zhkai
 * @date 2021年3月22日14:39:49
 */
public class Nio {
    /**
     * NIO 测试方法
     */
    public static void nioTest() {
        //同理对应的还有：ByteBuffer,IntBuffer,FloatBuffer,CharBuffer,ShortBuffer,DoubleBuffer,LongBuffer
        //创建一个Buffer，大小为5
        IntBuffer buffer = IntBuffer.allocate(5);
        // 存放数据
        int size = buffer.capacity();
        for (int i = 0; i < size; i++) {
            buffer.put(i);
        }
        // 切换成读模式，读写切换
        buffer.flip();
        while (buffer.hasRemaining()) {
            System.out.println(buffer.get());
        }
    }

}
```

### NIO 三大核心组件关系

![avatar](https://img.imgdb.cn/item/60583dcf8322e6675c703e73.png)

#### 说明

1. 每个 Channel 对应一个 Buffer。
2. Selector 对应一个线程，一个线程对应多个 Channel。
3. 该图反应了有三个 Channel 注册到该 Selector。
4. 该程序切换到哪个 Channel 是由事件决定的（Event）。
5. Selector 会根据不同的事件，在各个通道上切换。
6. Buffer 就是一个内存块，底层是一个数组。
7. 数据的读取和写入是通过 Buffer，但是需要 flip()切换读写模式。而 BIO

### NIO 三大核心理解

![avatar](https://img.imgdb.cn/item/605844f48322e6675c747b0b.jpg)

#### Buffer 的机制及子类

##### Buffer（缓冲区）基本介绍

缓冲区本质上是一个可以读写数据的内存块，可以理解为是一个容器对象（含数组），该对象提供了一组方法，可以更轻松的使用内存块，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。
Channel 提供从文件、网络读取数据的渠道，但是读取或者写都必须经过 Buffer。
在 Buffer 子类中维护着一个对应类型的数组，用来存放数据：

```Java
public abstract class IntBuffer
    extends Buffer
    implements Comparable<IntBuffer>
{

    // These fields are declared here rather than in Heap-X-Buffer in order to
    // reduce the number of virtual method invocations needed to access these
    // values, which is especially costly when coding small buffers.
    //
    final int[] hb;                  // Non-null only for heap buffers
    final int offset;
    boolean isReadOnly;                 // Valid only for heap buffers

    // Creates a new buffer with the given mark, position, limit, capacity,
    // backing array, and array offset
    //
    IntBuffer(int mark, int pos, int lim, int cap,   // package-private
                 int[] hb, int offset)
    {
        super(mark, pos, lim, cap);
        this.hb = hb;
        this.offset = offset;
    }

    // Creates a new buffer with the given mark, position, limit, and capacity
    //
    IntBuffer(int mark, int pos, int lim, int cap) { // package-private
        this(mark, pos, lim, cap, null, 0);
    }
}
```

| Buffer 常用子类 | 描述                   |
| --------------- | ---------------------- |
| ByteBuffer      | 存储字节数据到缓冲区   |
| ShortBuffer     | 存储字符创数据到缓冲区 |
| CharBuffer      | 存储字符数据到缓冲区   |
| IntBuffer       | 存储整数数据到缓冲区   |
| LongBuffer      | 存储长整型数据到缓冲区 |
| DoubleBuffer    | 存储浮点型数据到缓冲区 |
| FloatBuffer     | 存储浮点型数据到缓冲区 |

Buffer 中定义了四个属性来提供所有其包含的数据元素。

```Java
  // Invariants: mark <= position <= limit <= capacity
  private int mark = -1;
  private int position = 0;
  private int limit;
  private int capacity;
```

| 属性     | 描述                                                                                 |
| -------- | ------------------------------------------------------------------------------------ |
| capacity | 容量，即可以容纳的最大数据量；在缓冲区被创建时就被指定，无法修改                     |
| limit    | 表示缓冲区的当前终点，不能对缓冲区超过极限的位置进行读写操作，但极限是可以修改的     |
| position | 当前位置，下一个要被读或者写的索引，每次读写缓冲区数据都会改变该值，为下次读写做准备 |
| Mark     | 标记当前 position 位置，当 reset 后回到标记位置                                      |

#### Channel 的基本介绍

NIO 的通道类似于流，但有如下区别：

1. 通道是双向的可以进行读写，而流是单向的只能读，或者写。
2. 通道可以实现已读读取数据。
3. 通道可以从缓冲区读取数据，也可以写入数据到缓冲区。

常用的 Channel 有：FileChannel、DatagramChannel、SocketChannel、SocketServerChannel

##### FileChannel 类

FileChannel 主要用来对本地文件进行 IO 操作，常见的方法有：

1. 使用 FileChannel 写入文本文件

   ```Java
   /**
     * 使用FileChannel写入文本文件
     */
    public static void nioFileChannelWrite() throws IOException {
        String writeStr = "Hello, Java菜鸟程序员";
        // 创建一个输出流
        FileOutputStream fileOutputStream = new FileOutputStream("hello.txt");
        // 获取通道
        FileChannel channel = fileOutputStream.getChannel();
        // 创建缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate(100);
        // 写入byteBuffer
        byteBuffer.put(writeStr.getBytes());
        // 切换模式
        byteBuffer.flip();
        // 写入通道
        channel.write(byteBuffer);
        // 关闭
        channel.close();
        fileOutputStream.close();
    }
   ```

2. 使用 FileChannel 读取文本文件

   ```Java
   /**
     * 使用FileChannel 读取文本文件
     */
    public static void nioFileChannelRead() throws IOException{
        FileInputStream fileInputStream = new FileInputStream("hello.txt");
        FileChannel channel = fileInputStream.getChannel();
        ByteBuffer byteBuffer = ByteBuffer.allocate(100);
        channel.read(byteBuffer);
        System.out.println(new String(byteBuffer.array(), 0, byteBuffer.limit()));
        channel.close();
        fileInputStream.close();
    }
   ```

3. 使用 FileChannel 复制文件

   ```Java
    /**
     * 使用FileChannel复制文件
     */
    public static void nioFileChannelCopy() throws IOException{
        FileInputStream fileInputStream = new FileInputStream("hello.txt");
        FileOutputStream fileOutputStream = new FileOutputStream("world.txt");
        FileChannel inChannel = fileInputStream.getChannel();
        FileChannel outChannel = fileOutputStream.getChannel();
        ByteBuffer byteBuffer = ByteBuffer.allocate(1);
        while (inChannel.read(byteBuffer) != -1){
            byteBuffer.flip();
            outChannel.write(byteBuffer);
            // 清空重置
            byteBuffer.clear();
        }
        fileOutputStream.close();
        fileInputStream.close();
    }
   ```

4. 使用 transferFrom 复制文件

   ```Java
   /**
     * 使用transferFrom复制文件
     */
    public static void nioTransferFromCopy() throws IOException{
        FileInputStream fileInputStream = new FileInputStream("hello.txt");
        FileOutputStream fileOutputStream = new FileOutputStream("world.txt");
        FileChannel inChannel = fileInputStream.getChannel();
        FileChannel outChannel = fileOutputStream.getChannel();
        // 从哪拷贝，从几开始到几结束 对应的还有transferTo()方法
        outChannel.transferFrom(inChannel, 0, inChannel.size());
        outChannel.close();
        fileOutputStream.close();
        fileInputStream.close();
    }
   ```

##### Channel 和 Buffer 的注意事项

1. ByteBuffer 支持类型化的 put 和 get，put 放入什么数据类型，get 就应该使用相应的数据类型来取出，否则可能会产生 ByteUnderflowException 异常。
2. 可以将一个普通的 Buffer 转换为只读的 Buffer：asReadOnlyBuffer()方法。
3. NIO 提供了 MapperByteBuffer，可以让文件直接在内存（堆外内存）中进行修改，而如何同步到文件由 NIO 来完成。
4. NIO 还支持通过多个 Buffer（即 Buffer 数组）完成读写操作，即 Scattering（分散）和 Gathering（聚集）。
   - Scattering（分散）：在向缓冲区写入数据时，可以使用 Buffer 数组一次写入，一个 Buffer 数组写满后，继续写入下一个 Buffer 数组。
   - Gatering（聚集）：从缓冲区读取数据时，可以依次读取，读完一个 Buffer 再按顺序读取一下个。

#### Selector 的基本介绍

1. Java 的 NIO 使用了非阻塞的 I/O 方式。可以用一个线程处理若干个客户端连接，就会使用到 Selector（选择器）。
2. Selector 能够检测到多个注册通道上是否有事件发生（多个 Channel 以事件的形式注册到同一个 selector），如果有事件发生，便获取事件然后针对每个事件进行相应的处理。
3. 只有在连接真正有读写事件发生时，才会进行读写，减少系统开销，并且不必为每个连接都创建一个线程，不用维护多个线程。
4. 避免了多线程之间上下文切换导致的开销。

##### Selector 特点

Netty 的 I/O 线程 NioEventLoop 聚合了 Selector（选择器/多路复用器），可以并发处理成百上千个客户端连接。

当线程从某个客户端 Socket 通道进行读写时，若没有数据可用，该线程可以进行其他任务。

线程通常将非阻塞 I/O 的空闲时间用于其他通道上执行 I/O 操作，所以单独的线程可以管理多个输入输出通道。

由于读写操作都是非阻塞的，就可以充分提高 I/O 线程的运行效率，避免由于频繁 I/O 阻塞导致的线程挂起。

一个 I/O 线程可以并发处理 N 个客户端连接和读写操作，这从根本解决了传统同步阻塞 I/O 一连接一线程模型，架构性能、弹性伸缩能力和可靠性都得到极大的提升。

##### Selector 常用方法

```Java
public abstract class Selector implement Closeable{
  public static Selector open(); // 得到一个选择器对象
  public int select(long timeout); // 监听所有注册的通道，当其中的IO操作可以进行时，将对应的selectionkey加入内部集合并返回，参数设置超时时间
  public Set<SelectionKey> selectionKeys(); // 从内部集合中得到所有的SelectionKey
}
```

##### Selector 相关方法说明

- selector.select()：若监听到注册管道中有事件，则持续阻塞
- selector.select(1000)：阻塞 1000 毫秒，1000 毫秒后返回
- selector.wakeup()：换线 selector
- selector.selectNow()：不阻塞，立即返回

#### NIO 非阻塞网络编程过程分析

1. 当客户端连接时，会通过 SeverSocketChannel 得到对应的 SocketChannel。
2. Selector 进行监听，调用 select()方法，返回注册该 selector 的所有通道中有事件发生的通道个数。
3. 将 socketChannel 注册到 Selector 上，`public final SelectionKey register(Selector sel, int ops)`，一个 selector 上可以注册多个 SocketChannel。
4. 注册后返回一个 SelectionKey，会和该 Selector 关联（以集合的形式）。
5. 进一步得到各个 SelectionKey，有事件发生。
6. 再通过 SelectionKey 反向获取 SocketChannel，使用 channel()方法。
7. 可以通过得到的 channel，完成业务处理。

SelectionKey 中定义了四个操作标志位：OP_READ 表示通道中发生读事件；OP_WRITE 表示通道中发生写事件；OP_CONNECT 表示建立连接；OP_ACCEPT 表示请求连接。

##### SelectionKey 相关方法

| 方法                                               | 描述                         |
| -------------------------------------------------- | ---------------------------- |
| public abstract Selector selector();               | 得到与之关联的 Selector 对象 |
| public abstract SelectableChannel channel();       | 得到与之关联的通道           |
| public final Object attachment();                  | 得到与之关联的共享数据       |
| public abstract SelectionKey interestOps(int ops); | 设置或改变监听的事件类型     |
| public final boolean isReadable();                 | 通道是否可读                 |
| public final boolean usWritable();                 | 通道是否可写                 |
| public final boolean isAcceptable();               | 是否可以建立连接 ACCEPT      |

### NIO 和 BIO 对比

1. BIO 以流的方式处理数据，而 NIO 以块的方式处理数据，块 I/O 的效率比 I/O 高很多。
2. BIO 是阻塞的，而 NIO 是非阻塞的。
3. BIO 基于字节流和字符流进行操作，而 NIO 基于 Channel（通道）和 Buffer（缓冲区）进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区中写入到通道中。Selector（选择器）用于监听多个通道事件（比如连接请求，数据到达等），因此使用单个线程就可以监听多个客户端通道。

### AIO 基本介绍

JDK7 引入了 ASynchronous I/O，即 AIO。在进行 I/O 编程中，通常用到两种模式：Reactor 和 Proactor。Java 的 NIO 就是 Reactor，当有事件触发时，服务端得到通知，进行相应的处理。

AIO 叫做异步非阻塞的 I/O，引入了异步通道的概念，采用了 Proactor 模式，简化了程序编写，有效的请求才会启动线程，特点就是先由操作系统完成后才会通知服务器端程序启动线程去处理，一般用于连接数较多且连接时长较长的应用。

Reactor 和 Proactor

- 两种 IO 多路复用方案：Reactor and Proactor。
- Reactor 模式是基于同步 I/O 的，而 Proactor 模式是和异步 I/O 相关的。
