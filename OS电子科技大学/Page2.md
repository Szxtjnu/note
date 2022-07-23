### OS的演变、类型以及特征

##### Fixes(修改)

##### New Services

##### Hardware Upgrade Plus New Types of Hardware

驱动程序

##### Efficiency(效率)

#### Serial Processing （串行处理）

- No operating system
- Machines run for a console with display lights and toggle(切换) switches, input device, and printer. Two main problems:
	- 调度十分的浪费时间 waste processing time
	- Setup time: Setup included loading the compiler（加载编译器）, source program, saving compiled program, and loading and linking.

##### Simple Batch System(简单批处理系统)

- Monitors(监督程序)：Software that controls the running programs
	- Resident monitor is in main memory and available for execution
	- 是常驻内存的，当内存没有作业时，从Disk中选出一个作业来，调入内存来进行操作，进入的作业占据了内存的全部空间，运行结束之后，Monitor负责将空间清空，准备进行下一个作业。
- Batch jobs together（批处理作业）
- Program branches back to monitor when finished

#### Uniprogramming（单道程序系统）

- Peocessor must wait for I/O instruction to complete before proceeding.

#### Multiprogramming（多道程序系统）

When one job needs to wait for I/O, the processor can switch to the other job.

允许多个程序同时准备运行，即进入内存后还需要排队，需要调度。

#### Difficulties with Multiprogramming

- Improper synchronization（同步）不合适的同步
	- ensure a process waiting for an I/O device receives the signal
- Failed mutual exclusion（互斥）
- Nondeterminate（不确定性） program operation
- Deadlocks（死锁）

#### Time Sharing（分时系统）

- Using multiprogramming to handle（解决） multiple interactive jobs.
- Processor's time is shared among multiple（许多的） users
- Multiple users simultaneously（同时 ）access the sytem through terminals.（终端）

> 多道批处理系统和分时系统的比较
>
> 多道批处理系统主要是要求尽可能的最大程度上使用处理器，将其使用率达到最大
>
> 分时系统主要是要求的响应时间，响应时间要尽量短。

#### 现代OS的基本类

按照硬件平台系统结构分类：

- 单机OS

	按照功能特征分类：

	- Batch_Processing OS 吞吐量大
	- Time_Sharing OS
	- Real_Time OS
		- 无人驾驶（实时控制系统）
		- 售票系统（实时信息处理系统）

- 并行OS

- 网络OS

- 分布式OS

	- DNS服务器

#### 现代OS的两个基本特征

- 任务共行
	- 从宏观上来看，任务共行是指系统中有多个任务同时运行。
	- 从微观上来看，任务共行是指单处理机系统中的任务并发(Task Concurrency)或多处理机系统中的任务并行(Task Parallelism)
- 资源共享
	- 从宏观上来看，资源共享是指多个任务可以同时使用系统中的软硬件资源
	- 从微观上来看，资源共享是指多个任务可以<u>*交替互斥*</u>地使用系统中的某个资源

#### 任务管理模型

Task是指计算机系统在某个资源集合上做的一次相对独立的计算过程

现代OS中，任务用<u>*线程和进程*</u>这两个基本概念共同表示

传统OS中，任务仅仅用<u>*进程*</u>这一基本概念表示。

### OS Architecture（结构风格）

#### 一种常见的OS的总体结构风格

两类子系统

1. 用户接口子系统——提供计算机用户需求的用户命令
2. 基础平台子系统——提供软件需求的系统调用

用户接口子系统和基础平台子系统之间的相互关系具有单向性。即用户平台子系统对于基础平台子系统是一种调用关系，反过来说，基础平台子系统为用户平台提供一个基础。

#### OS基础平台子系统结构风格（一）

- Layered Structural Style（分层结构风格）
	- 使用分层结构风格，包含若干层。其中每一层实现一组基本概念以及相关的基本属性
	- 层与层之间的关系
		- 所有各层之间不依赖以上各层所提供的概念及其属性，只依赖其直接下层所提供的的概念以及属性；
		- 每一层均对其上各层隐藏其下各层的存在
- Hierarchical Structrual Style（分级结构风格）
	- 使用分级结构的基础平台子结构包含若干level
	- 级与级之间的关系
		- 所有各级的实现不依赖其以上各级所提供的概念以及属性，只依赖其以下各级所提供的概念及属性
- Modular Structural Style（分块结构风格）
	- 使用分块结构风格的基础平台子结构包含若干module
	- 块与块之间的相互关系
		- 所有各块的实现均可以任意引用其他各块所提供的概念以及属性

##### 分层、分级、分块结构风格的关系：

- 分层结构是一种特殊的分级结构
- 分级结构是一种特殊的分块结构

##### 分层、分块结构风格的比较

- 分层结构风格
	- 有利于实现基础平台的子系统的可维护性、可拓展性
	- 不利于提高基础平台子系统的时间和空间效率
	- 构造一个纯粹的分层结构十分困难
- 分块结构风格
	- 构造一个分块结构是一种切换实际的做法
	- 有利于生成高效、紧凑的基础平台子系统可执行代码
	- 不利于实现基础平台子系统的灵活性

> 分级结构风格的长处和不足介于两者之间

#### OS基础平台子系统结构风格（二)

- Multi-Mode Structural Style （多模式结构风格）
- Single-Mode Structural Style （单模式结构风格）

##### 基本概念：模式（Mode）

所谓<u>模式</u>，简单地说就是程序在运行过程中使用的、由硬件体系结构提供的CPU特权模式。

##### 多模式结构风格的特征

使用多模式结构风格，子系统将包含多个模式模块，这些模式模块或是一个应用软件或是基础平台子系统的一部分

##### 单模式结构风格的结构特征

使用单模式结构风格，子系统将仅仅包含一个模式模块，该模式模块由应用软件和基础平台子系统共同组成。

使用单模式结构风格的基础平台子系统结构中，应用程序和基础平台子系统在同一CPU特权模式下运行。

##### 多模式与单模式比较

- 多模式
	- 有利于实现可靠性、安全性等非功能性需求
	- 会降低基础平台子系统的性能
	- 在较高级别CPU特权模式下调试程序是一件特别困难的事情
- 单模式
	- 不会增加基础平台子系统的开发难度
	- 不会影响基础平台子系统的性能
	- 不利于实现基础平台子系统的可靠性、安全性等非功能性需求

##### 双模式基础平台子系统

若一个基础平台子系统使用了双模式结构风格，则称该基础平台子系统为双模式基础平台子系统。

包含两种模式，分别在两种不同的CPU特权模式下运行。习惯上，这两个模式分别成为核外子系统(User Mode)和核心子系统(Kernel Mode)。

> User mode is Less-privileged mode. 
>
> 用户模式下运行的程序之间是可以相互调用，相互访问数据的，是平等的。但是用户程序需要调用系统级的数据时候，需要判断是否是合法访问。
>
> System mode, control mode, or kernel mode is more-privileged mode.
>
> 应用程序需要调用系统服务时，需要切换模式，从低权限到高权限的切换，才能完成系统的工作。<u>这里需要进行判定，系统需要进行这样的工作——软中断</u>。软中断相比硬中断来说，不需要停止CPU的运行。

##### Microkernels（微核）结构

设计思想：尽最大努力剔除核心子系统中的多余成分，并把它们移到核外子系统中实现，核心子系统中仅需要一些必要的简单的概念以及其属性，从而保证核心子系统的简洁高效。

Small operating system core.

Contains only essential operating systems functions.