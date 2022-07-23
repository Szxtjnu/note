### OS的系统需求

#### 软件系统的需求分析

1. 软件系统能提供的服务（功能性需求）
2. 软件系统在提供这些服务的时候，需要满足的限制条件（非功能性需求）
3. 软件系统具有适应某些变换的能力（非功能性需求）

#### 操作系统的功能性需求

- 计算机用户需要的用户命令

- 应用程序需要的System Call （系统调用）

	由操作系统实现的所有系统调用所构成的集合被人们称为程序接口或应用编程接口(Application Programming Interface)

##### Interface 

- 用户命令
- 命令的表示形式
	- 字符形式
	- 菜单形式
	- 图形形式
- 命令的使用方式
	- 脱机使用方式(off- line)
	- 联机使用方式(on - line)

##### System Call

指由OS实现的应用程序在运行过程中可以引用的System Service

常用的API：POSIX.1 \ WIN32 API 

#### 非功能性需求

##### Performance（性能）or Efficiency（性能）

吞吐量(maximize throughput)，也就是说单位时间的系统完成任务。即在限定时间内完成的任务越多越好，也就是说吞吐量越大越好。

响应时间(minimize response time)

可支持的用户数

##### Fairness

give equal and fair access to all processes

##### Reliability(可靠性)

##### Security（安全性）

##### Scalability(可伸缩性)

##### Extensibility（可扩展性）

#### OS对硬件平台的依赖

##### Timer

##### I/O Interrupts

##### DMA or Channel

##### Privileged Instructions（特权指令）——进程的并发

##### Memory Protection Mechanism

操作系统所提供的机制需要这些硬件平台的基础

#### Job

作业，是指计算机用户在上机的过程中要求计算机为其工作的集合；作业中每项相对独立的工作成为**作业步**。Job Control Language 类似于批处理文件。

#### Thread & Process

Thread 是指，程序的一次相对独立的运行过程；在现代OS中，线程是<u>系统调度的最小单位</u>。

Process 是指，系统分配资源的基本对象；在现代OS中，进程仅仅是系统中<u>拥有资源的最小实体</u>；不过在传统的OS中，进程同时也是系统调度的最小单位。

#### Virtual Memory

简单的说，就是进程的**逻辑地址**空间；它是现代OS对计算机系统中多级物理存储体系进行高度抽象的结果。将外存的一部分虚拟为内存来使用，变相的扩大了内存空间。

#### File

简单的说，就是命名了的字节流。