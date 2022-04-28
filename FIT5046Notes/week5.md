# Week 5 Notes

## Thread

Process 进程:

- 每一个Android App 打开的时候，JVM提供一个进程并且下辖一个进程

- 每个进程下可以容纳多个线程

Thread 线程:

- 当App启动的时候，JVM提供main线程供MainActivity运行

- 也叫做主线程 == UI线程

- 线程的两条黄金定律:

  - 永远不要block UI线程(主线程) -- 新建线程后台运行
  - 永远不要从UI线程以外访问UI控件

## Room

- Room在SQLite上提供了一个抽象层，以便在发挥SQLite能力的同时允许流畅的数据库访问。
- 针对SQLite的封装
- 使用简便
- 是耗时操作，**不能在UI Thread使用**

### 使用Room创建数据库的步骤

1. extends RoomDB
2. declaration DAO
3. init thread pool
4. create Room DB instance

