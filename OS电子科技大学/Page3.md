# 进程与调度

## 进程的描述与控制

### 程序的执行顺序

- 程序顺序执行
	- 顺序性、封闭性、可再现性
- 程序并发执行
	- 间断性、非封闭性、不可再现性
- 程序并发执行条件(Brenstein条件)

### 进程的概念与进程的并发执行

#### Process 

- Also called a task

- Execution (执行) of an individual (单独的) program

- Can be traced (跟踪)
	- list the sequence of (一连串的) instructions that execute（执行方法）

##### Characteristics of Process

- Dynamic (动态性)
- Concurrency (并发性)
- Independent
- Asynchronous (异步性)

##### Process Structure

- Programs
- Data
- PCB (Process Control Block)

> 例：假设内存中有3个进程A、B、C，他们的程序代码已全部装入内存。若A、C两进程需要执行12条指令，B进程需要执行4条指令，且B进程执行到第4条指令处必须等待I/O。如何跟踪他们的执行过程？ 

<img src="https://pic.imgdb.cn/item/62277ef35baa1a80ab96ae2d.jpg" style="zoom: 67%;" />

Dispatcher(分派)等同于 scheduler(调度程序) CPU的监管程序 (Monitor)

A先进入内存，然后是B，最后是C。

<img src="https://pic.imgdb.cn/item/6244694327f86abb2abc9211.jpg" style="zoom:67%;" />

#### Two-State Process Model

Process may be in one of two states

- running
- not-running

> 注：
>
> 并非所有进程只要Not-running就处在ready的状态，有的时候需要blocked，等待IO完成
>
> Not-running 又可以分为 **ready && blocked**两种状态，即就绪态和阻塞态

#### A Five-State Model

- Running (执行态)
	- 占用处理机（单处理机环境中，某一时刻仅一个进程占用处理机）
- Ready (就绪态)
	- 准备执行
- Blocked (阻塞态)
	- 等待某事件发生才能执行，如等待IO完成
- New (新状态)
	- 进程已经创建，但未被OS接纳为可执行进程
- Exit (退出态)
	- 因停止或取消，被OS从执行状态解放

![](https://pic.imgdb.cn/item/62446cc627f86abb2ac53568.jpg)

- New -> Ready 进程被接纳
- Ready -> Running 就绪态的进程被分派到系统资源
- Running -> Blocked 由于需要等待事件的发生
- Blocked -> Ready 需要等待的事件已经发生
- Running -> Ready 分时系统中，时间片用完 || 优先级高的进程到来，终止优先级低的进程的执行
- Running -> Exit 进程执行完毕，释放资源后进入Exit态 || 被系统取消
- Ready -> Exit 某些系统允许父进程在任何情况下终止其子进程。

#### Using Two Queues

![](https://pic.imgdb.cn/item/62446f6927f86abb2acba4c4.jpg)

**由于进程进入等待状态的原因并不相同，所以我们需要设立多个不同事件的阻塞队列**

![](https://pic.imgdb.cn/item/6244705527f86abb2acdcb41.jpg)

---

### Swapping (对换技术)

将内存中暂时不能运行的进程，或暂时不用的数据和程序，Swapping-out到外存，以腾出足够的内存空间，把已具备运行条件的进程，或进程所需要的数据和程序，Swapping-in 内存

#### Suspended Process (被挂起进程)

Processor is faster than I/O so all process could be waiting for I/O. In other word, the processor is free now.

Swap these processes to disk to free up more memory. 

Blocked state becomes suspend state when swapped to disk. 阻塞态会变成挂起态

##### Reasons for Process Suspension

- Swapping 
	- The operating system needs to release sufficient (充足的) main memory to bring in a process that is ready to execute (执行).
- Other OS reason
	- The operating system may suspend a background or utility(实用的) process or a process that is suspected (可能) of causing a problem.
	- 例如某些系统中检查死锁、系统碎片的进程，是周期性运行的，当他们的周期没有到来的时候，是挂起状态的。
- Interactive (交互) user request
	- A user may wish to suspend execution of a program for purposes of debugging or in connection with the use of a resource.
	- 用户在使用计算机的时候可以强制挂起一个进程
- Timing
	- 时钟中断或父进程挂起子进程

##### 被挂起进程的特征

- 不能立即执行

- 可能是等待某事件发生。若是，<u>则阻塞条件独立于挂起条件</u>，即使阻塞事件发生，该进程也不能执行
- 使之挂起的进程为：自身，其父进程，OS
- 只有挂起它的进程才能使之由挂起状态转换为其他状态

#### Suspend vs. Blocked

Question:

1. 是否只能挂起阻塞进程？

	不一定，优先挂起阻塞进程，因为阻塞进程在内存中

2. 如何激活一个挂起进程？

	类似于I/O过程

？进程是否等待事件，*<u>阻塞与否</u>*

？进程是否被换出内存，*<u>挂起与否</u>*

##### 4种状态组合

Ready:进程在内存，准备执行

Blocked:进程在内存，等待事件

Ready, Suspend:进程在外存，只要调入内存即可执行

Blocked, Suspend:进程在外存，等待事件

> 注：处理机可调度执行的进程有两种：
>
> 1. 新创建的进程
>
> 2. 或换入一个以前挂起的进程
>
> 	通常为避免增加系统负载，系统会换入一个以前挂起的进程执行

##### 7种系统状态转换图

![](https://pic.imgdb.cn/item/62447d6e27f86abb2aeb314b.jpg)