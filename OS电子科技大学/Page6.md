#### Thread(线程)

An execution state

Saved thread context when not running.

Has an execution stack.(执行堆栈)

Some per-thread static storage for local variables.

Access to the memory and resources of its process.

> 进程是资源分配单位，线程是调度单位，线程共享的是进程的资源。**减小系统开销**

##### 单进程单线程模型和单进程多线程模型

![](https://pic.imgdb.cn/item/624d386b239250f7c564a18e.jpg)

##### Benefits of Threads

- Takes less time to create  a new thread than a process.
- Less time to terminate a thread than a process.（线程终止不需要释放资源）
- **Less time to switch between two threads within the same process.**（一个进程之间的线程切换会更加节省时间）
- Since threads within the same process share memory and files, they can communicate with each other without invoking（激发） the kernel.（不需要进行模式的切换）

##### Character of Threads

Suspending a process involves suspending all threads of the process. Since all threads share the same address space.

Termination of a process, terminates all threads within the process.

##### Multithreading

- Operating system supports multiple threads of execution within a single process.
- MS-DOS supports a single thread.
- UNIX supports multiple user processes but only supports one thread per process.
- Windows 2000, Solaris, Linux, Mach, and OS/2 support multiple threads.

##### Thread States

- Key states for a thread: Running, Ready, Blocked
- Operations associated with a change in thread state.
	- Spawn(派生), Spawn another thread
	- Block
	- Unblock
	- Finish

##### User-Level Threads

- All thread management is done by the application.
- The kernel is not aware of the existence of threads.
- 用户级线程只有用户看得见，内核是不知道的
- 描述此类线程的数据结构以及控制此类线程原语在核外子系统中实现

##### Kernel-Level Threads

- W2K, Linux, and OS/2 are examples of this approach.
- Kernel maintains context information for the process and the threads.
- Scheduling is done on a thread basis.
- 描述此类线程的数据结构以及控制此类线程的原语在核心子系统中实现

##### Combined Approaches

- Example is Solaris.
- Thread creation done in the user space.
- 既有内核级的线程又有用户级的线程

![](https://pic.imgdb.cn/item/624d4118239250f7c5762044.jpg)

| Threads : Process | Description                                                  | Example Systems                  |
| ----------------- | ------------------------------------------------------------ | -------------------------------- |
| 1 : 1             | Each thread of execution is a unique process with its own address space and resources | Traditional UNIX implementations |
| M : 1             | A process defines an address space and dynamic resource ownership. Multiple threads may be created and executed within that process. | Windows NT, Linux, Solaris, OS/2 |

---

### 进程的调度

#### Type of scheduling

##### Aim of Scheduling 

- Response time (响应时间)
- Throughput (系统吞吐量)
- Processor efficiency (处理机效率)
- Fairness (公平性，防止进程饥饿)

##### Type

- 按OS的类型分：

	批处理调度、分时调度、实时调度、多处理机调度

- 按照调度的层次分：

	Long-Term scheduling

	Medium-term scheduling

	Short-term scheduling

![](https://pic.imgdb.cn/item/624d4b3f239250f7c58b163a.jpg)

##### Long-term schduling

又称为高级调度、作业调度，它为被调度的作业或用户程序创建进程，分配必要的系统资源，并将新创建的进程插入就绪队列，等待Short-term scheduling

- Determines which programs are admitted to the system for processing
	- 取决于调度算法
- How many programs are admitted to the system?
	- Controls the degree of multiprogramming
- When does the scheuler be invoked
	- Each time a job terminates
	- Processor is idle (空闲) exceeds a certain threshold (阈值)

##### Medium-term scheduling

又称为中级调度，它调度换出到磁盘的进程进入内存，准备执行

- 中程调度配合对换技术使用
- 目的是为了提高内存的利用率和系统的吞吐量
- 在多道程序度允许的情况下，从外存中选择一个挂起状态的进程调度到内存（换入）

##### Short-term scheduling

- 又称为进程调度、低级调度，调度内存中的就绪进程执行

- Known as the dispatcher：决定就绪队列Which进程将获得处理机
- Executes most frequently
- Invoked when an event occurs
	- Clock interrupts
	- I/O interrupts
	- Operating system calls
	- Singnals

#### Scheduling Criteria (标准)

##### User - oriented（用户导向）

- Response Time (响应时间)
	- Elapsed time (运行时间) betwen the submission of a reqest until there is output.
	- 常用于评价分时系统的性能
- Turnaround time (周转时间)
	- 是指从作业提交给系统开始，到作业完成为止的这段时间间隔（也称为作业周转时间）
	- 常用于评价批处理系统的性能
- Deadlines (截止时间)
	- 是指某任务必须开始执行的最迟时间，必须完成的最迟时间
	- 常用于评价实时系统的性能

##### System - orinted (系统导向)

- Throughput (吞吐量)
	- 单位时间内系统所完成的作业数
	- 用于评论批处理系统的性能
- 处理机利用率
	- This is the percentage of time that the processor is busy.
	- Effective and efficient utilization (高效利用) of the processor.
- Balancing Resource (资源平衡)
	- Keep the resources of the system busy.
	- 适用于长程调度和中程调度
- Fairness (公平性)
	- Process should be treated the same, and no process should suffer starvation (饥饿进程).

##### Priorities

- Scheduler will always choose a processs of higher priority over one of lower priority.
- Have multiple Ready queues to represent each level of priority.
- Lower - priority may suffer starvation.
	- allow a process to change its priority based on its age or execution history.

---

### 调度算法 Scheduling Algorithms

#### Decision Mode

##### Nonpressmptive (非剥夺方式)

- Once a process is in the running state, it will continue until it terminates or blockes itself for I/O.
- 主要用于批处理系统

##### Preemptive (剥夺方式)

- Currently running process may be interrupted and moved to the Ready state by the operating system.
- Allows for better service since any on process cannot monopolize (独占) the processor for very long.
- 主要用于实时性要求较高的实时系统以及性能要求较高的批处理系统和分时系统。

#### Process Scheduling Example

<img src="https://pic.imgdb.cn/item/624e54aa239250f7c53581ba.jpg" style="zoom:50%;" />

分6种不同的短程调度算法调度这5个进程，比较各种调度算法

##### First - Come - First - Served

- Each process joins the Ready queue.
- When the current process ceases to execute, the oldest process in the Ready queue is selected.
- 非剥夺方式
- 计算平均周转时间
	- A short process may have to wait a very long time before it can execute.
	- Favors CPU-bound processes.
		- I/O processes have to wait until CPU-bound process completes.
- 一般，FCFS与其他的调度算法混合使用
- 系统可以按照不同的优先级维护多个就绪队列，每个队列内部按照FCFS算法调度。

##### Round Robin (轮转法，时间片法)

- Uses preemption (剥夺) based on a clock.

- An amount of time is determined that allows each process to use the processor for that length of time

	- Known as time slicing (时间片)

- Clock interrupt is generated at periodic (周期的)  intervals (间隔).

- When an interrput occurs, the currently running process is placed in the Ready queue.

	- Next ready job is selected.

- RR算法分析

	1. 时间片的大小对计算机性能的影响

	2. 存在的问题：

		未有效的利用系统资源。Processor-bound 进程充分利用时间片，而I/O -bound进程在两次I/O之间仅需要很少的CPU时间，却需要等待一个时间片

##### Virtual round robin

- RR算法的改进
	- 增加一个辅助队列，接受I/O阻塞完成的进程，调度优先于就绪队列, 但占用的处理机时间小于就绪队列的时间片
- 算法分析：VRR算法比RR算法公平
- RR算法常用于分时系统及事务处理系统

##### Short Process Next (SPN 短进程优先)

- Policy: Preemptive or Nonpreemeptive?
- Process with shortest expected processing time is selcted next.
- Short process jumps ahead of longer processes
- SPN算法分析：
	1. 改善了系统的性能，降低了系统的平均等待时间，提高了系统的吞吐量
	2. 存在的问题：
		1. 很难准确确定进程的执行时间
		2. 对长进程不公平，Possibility of starvation for long processes.
		3. 未考虑进程的紧迫程度，不适合于分时系统和事物处理系统

##### Shortest Ramaining Time (SRT，剩余最短时间者优先)

- Preemptive version of shortest process  next policy.
- Must estimate (估计) processing time.
- SRT算法分析：
	1. 与SPN算法一样，很难准确确定进程的剩余执行时间且对长进程不公平
	2. 但是，它不想FCFS算法偏袒长进程，也不想RR算法会产生很多中断而增加系统负担
	3. 由于短进程提前完成，故其平均周转时间比SPN短。

##### Highest Response Ratio Next (响应比高者优先)

- RR = (time spent waiting + expeceted service time) / expected service time
- Choose next process with the highest response ratio.
- HRRN算法分析
	1. 若进程的等待时间相同，则要求服务时间短的进程优先
	2. 若进程要求的服务时间相同，则等待时间长的进程优先
	3. 长进程随着等待时间的增加，必然会被调度
	4. 但是，很难准确的确定进程要求的执行时间，且每次调度之前需要计算响应比，增加了系统的开销

##### Feedback (反馈调度法)

1. 设置多个就绪队列，各队的优先权不同（从上而下优先级逐级降低）
2. 各个队列中进程执行的时间片不同，优先权越高时间片越短
3. 新进程首先放入第一队列尾，按
