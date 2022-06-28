# Java基础

## 字节码

在Java中，JVM可以理解的代码叫字节码（.class文件），它不面向任何特定的处理器，只面向虚拟机。JVM有针对不同系统的特定实现(Windows, Linux, macOS)， 目的是使用相同的字节码，它们会给出相同的结果。字节码和不同系统的JVM实现是Java实现“一次编译，随处可以运行”的关键所在

## Java和C++的区别

1. 都是面向对象的语言，支持继承封装多态
2. Java不提供指针来直接访问内存，程序内存更加安全
3. Java的类是单继承的，C++支持多继承；虽然Java不支持多继承，但是其接口可以多继承
4. Java有自动内存管理机制，不需要程序员手动释放无用内存

## 重载和重写

重载就是同样一个方法根据输入的数据不同，做出的处理不同。方法名相同，方法签名不同

重写就是当子类继承父类的相同方法，输入数据一样，但要做出有别于父类的响应时，就要覆盖父类的方法

区别：

| 区别点 | 重载 | 重写 |
| --- | --- | --- |
| 发生范围 | 同一个类 | 子类 |
| 参数列表 | 必须修改 | 一定不能修改 |
| 返回类型 | 可修改 | 子类方法返回值类型应比父类方法返回值类型更小或相等 |
| 异常 | 可修改 | 子类方法声明抛出的异常类型应比父类方法声明抛出的一场类更小或相等 |
| 访问修饰符 | 可修改 | 一定不能做更严格的限制（可以降低限制） |
| 发生阶段 | 编译期 | 运行期 |

## Java三大特性

### 封装

把一个对象的属性私有化，同时提供一些可以被外界访问的属性和方法

### 继承

使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或功能。子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问的，只是拥有

### 多态

指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定。在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中的同一方法）

## String, StringBuffer, StringBuilder

 String类中使用final关键字修饰字符数组来保存字符串，pivate final char value[ ], 所以String 对象是不可变的

**线程安全角度：** String对象是不可变的，可以理解为常量，线程安全；StringBuffer对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的；StringBuilder没对方法加同步锁，非线程安全

**性能角度：** 每次对String类型进行改变的时候，都会生成一个新的String对象，然后将指针指向新的String对象。StringBuffer每次会对StringBuffer对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下StringBuilder相比StringBuffer能获得10%-15%左右的性能提升，但是要冒多线程不安全的风险

**总结：** 
操作少量的数据：适用String
单线程操作字符串缓冲区下操作大量数据：适用StringBuilder
多线程操作字符串缓冲区下操作大量数据：适用StringBuffer

## 接口和抽象类

- 抽象类用extend, 接口用implement
- 抽象类可以有构造函数，接口不能有
- 抽象类有main方法并且可以运行，接口不能有
- 一个类可以实现多个接口，但只能继承一个抽象类
- 接口方法默认修饰符public，抽象方法可以有public， protected和default
- 从设计层面来说，抽象是对类的抽象，是一种设计模板；而接口是对行为的抽象，是一种行为的规范

## 获取键盘输入的两种常用方式

    //第一种
    Scanner input = new Scanner(System.in);
    String s = input.nextLine();
    input.close();

    //第二种
    BufferReader input = new BufferReader(new inputStreamReader(System.in));
    String s = input.readLine();

## == 和 equals

### ==

判断两个对象的地址是否相等。即，判断两个对象是否为同一个对象（基本数据类型==比较的是值，引用数据类型比较的是内存地址）

### equals

如果没有重写equals() 方法就通过该方法比较类的两个对象时，等价于通过“==”比较这两个对象；如果类中重写了equals()方法，一般我们把它重写为比较两个对象的内容是否相等，如果相等则返回true

## hashCode 和 equals

### hashcode

hashCode的作用是获取哈希码，即一个int整数，用来确定该对象在哈希表中的索引位置。
hashCode是定义在Object类中的方法，所以所有对象都有这个hashCode函数。虽然所有对象都有，但仅仅当创建某个**类的散列表（HashMap, HashTable, HashSet等）** 时，该类的hashCode才有作用（确定该对象在散列表中的位置；其他情况，如创建类的单个对象，或者创建类的对象数组等hashCode函数没有作用）

### 联系

分两种情况：

1. 与散列表无关
2. 与散列表相关

参考：https://www.cnblogs.com/skywang12345/p/3324958.html

### 自定义hashCode

- hashCode根据内存来计算，而类信息一般在方法区不会变动

## 为什么Java只有值传递

方法中的形参如果是基本类型，则复制数值；如果是对象引用，则复制引用，对复制的引用修改会影响到指向的对象，也就影响到了作为方法形参的实参；如果只是交换两个复制的引用，那么对实参没有影响

## Java序列化中有字段不想被序列化怎么办

使用transient关键字，作用是阻止实例中那些用此关键字修饰的变量序列化；当对象被反序列化时，被transient修饰的变量值不会被持久化和恢复。transient只能修饰变量，不能修饰类和方法

## 什么是反射机制

在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取信息以及动态调用对象的方法的功能称为Java的反射机制

## 反射机制的应用场景

### JDBC数据库连接

1. 通过Class.forName("com.mysql.jdbc.Driver")加载数据库的驱动程序（通过反射加载，前提是引入了相关jar包）
2. 通过DriverManager类进行数据库的连接，连接的时候要输入数据库的连接地址，用户名，密码
3. 通过connection接口接收连接

## Spring配置文件加载

1. 将程序内所有XML或Properties配置文件加载到内存中
2. Java类里解析XML或Properties里面的内容，得到对应实体类的字节码字符串以及相关的属性信息
3. 使用反射机制，根据这个字符串获得某个类的Class实例
4. 动态配置实例的属性

## Java中的IO流分为几种

### 按照流的流向分

#### 输入流

InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流：

- FileInputStream
- PipedInputStream
- ByteArrayInputStream
- BufferedInputStream
- DataInputStream

#### 输出流

OutPutStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流：

- FileOutputStream
- PipedOutputStream
- ByteArrayOutputStream
- BufferedOutputStream
- DataOutputStream

### 按操作单元划分

分为字节流和字符流

### 按流的角色划分

分为节点流和处理流

## 有了字节流为什么还要有字符流

因为字符流是由字节流转换得到的，这个过程非常耗时，而且不知道编码问题很容易出现乱码问题。所以，I/O流就干脆提供了一个直接操作字符的接口。音频，图片等媒体文件用字节流比较好，涉及到字符的用字符流比较好

## BIO，NIO，AIO区别

### BIO（Blocking I/O）

同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的I/O且编程模型简单，也不用过多考虑系统的过载，限流等问题

### NIO（Non-blocking/New I/O）

NIO是一种同步非阻塞的I/O模型，NIO提供了与传统BIO模型中的Socket和ServerSocket相对应的SocketChannel和ServerSocketChannel两种不同的套接字通道实现，两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式刚好相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用NIO的非阻塞模式来开发

### AIO（Asynchronous I/O）

异步非阻塞IO是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会阻塞在那里，当后台处理完成时，操作系统会通知相应的线程进行后续的操作。AIO是异步IO的缩写，虽然NIO在网络操作中提供了非阻塞的方法，但是NIO的IO行为还是同步的

### 烧水壶例子

烧一排水壶，BIO就是烧完一壶才可以烧下一壶，NIO就是有一个人循环看着一排水壶的状况，有情况通知，AIO就是每个水壶自己烧开了通知对应的线程处理；同步和异步的区别就是一个消息发送是否需要等上一次消息返回；阻塞和非阻塞的区别就是等待结果时是否会被阻塞

## 深拷贝和浅拷贝

### 浅拷贝

只拷贝被拷贝对象本身，而其他所有对象的引用仍然指向原来的对象。即，只会对“主”对象进行拷贝，不会拷贝主对象里的对象

### 深拷贝

当被拷贝对象本身以及其引用的对象一起拷贝时发生了深拷贝

### 参考

https://blog.csdn.net/baiye_xing/article/details/71788741

## Java异常

### Throwable

#### Error

是程序无法处理的错误，表示运行应用程序中有严重的问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时JVM出现的问题。例如JVM运行错误（Virtual MachineError），当JVM不再有继续执行操作所需的内存资源时，将出现OutOfMemoryError。这些异常发生时，JVM一般会选择线程终止。

#### Exception

是程序本身可以处理的异常。Exception类有一个重要的子类RuntimeException。RuntimeException异常由JVM抛出。

- NullPointerException：要访问的变量没有引用任何对象
- ArthmeticException：算数运算异常
- ArrayIndexOutOfBoundException：下标越界

### 异常处理

#### try

用于捕获异常，将可能产生异常的代码包住

#### catch

用于对捕获的异常进行处理

#### finally

无论是否有异常捕获都会执行其中的代码。当try或catch中有return时，finally中的语句会在返回前执行；若finally中也有return，则其将会覆盖try或catch中的返回值

#### 四种finally语句不会执行的特殊情况

1. 在finally语句块第一行发生了异常。因为在其他行，finally块还是会得到执行
2. 前面的代码用了System.exit(int)退出程序，如果在此之前发生了异常，还是会执行
3. 程序所在线程死亡
4. 关闭CPU

## Java1.8新特性

### Lambda表达式

允许把函数作为一个方法的参数

### 方法引用

允许直接引用已有Java类或对象的方法或构造方法

    ArrayList<String> list = new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.forEach(System.out::println);

### 接口允许定义默认方法和静态方法

允许接口中存在一个或多个默认非抽象方法和静态方法
