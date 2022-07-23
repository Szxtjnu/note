### Typical Function of an OS Kernel

#### 资源管理功能

- Process Management：进程创建和终止、调度、状态转换、同步和通信、管理PCB
- Memory Management：为进程分配地址空间、对话、段/页管理
- I/O Management：缓存管理、为进程分配I/O通道和设备

#### 支撑功能

- Interrupt handling（中断处理）
- Timing（时钟管理）
- Primitive （原语）：Atomic Operation；执行过程不可中断
- Accounting （统计）
- Monitoring（监测）

### Process Control Primitives (原语)

- Process Switch，进程切换
- Create and Terminate，创建与终止
- Block and Wakeup, 阻塞与唤醒
- Suspend and Activate,挂起与激活

#### When to Switch a Process

- Clock interrupt
	- Process has executed (执行) for the maximum allowable time slice (时间片). 
- I/O interrupt
- Memory fault (存储访问失效)
	- memory address is in virtual memory so it must be brought into main memory.
- Trap
	- error occurred
	- may cause process to be moved to Exit state
- Supervisor call (监管程序调入)
	- such as file open

##### Change of Process State

1. <u>Save context</u>(上下文) of processor including program counter and other registers (记录)

	切换进程时候需要保存信息到PCB中

2. Update the PCB of the process that is currently running.

3. Move PCB to appropriate queue - ready, blocked

4. Select another process for execution

5. Update the PCB of the peocess selected

6. Update memory-management data structures

7. <u>Restore context</u> of the selected process

##### Process Swithing *vs.*Mode Switching

- Process Switch, 是作用于进程之间的一种操作。当分派程序收回当前进程的CPU并准备把它分派给某个就绪进程时，该操作将被引用
- Mode Switch, 是进程内部所引用的一种操作。当进程映像所包含的程序引用核心子系统所提供的系统调用时，该操作将被引用

> 最重要的区别就是模式切换是进程内部使用的，不涉及到进程的切换，而进程切换是相对于两个不同的进程而言的。
>
> 那么进程切换中是否有模式切换？
>
> 进程切换中是有模式切换存在的，理由是进程切换的时候需要调度程序的参与，而调度程序是系统态下的程序，且进程切换的时候需要中断，而中断也是在系统态下面执行的。

#### Process Creation (原语)

##### Process Creat

- Submission (提交) of a batch job
- User logs on 
- Created to provide a service such as printing
- Process creates another process

##### creat原语步骤

1. 为进程分配一个唯一标识号ID：主进程表中增加一个新的表项
2. 为进程分配空间：用户地址空间、用户栈空间、PCB空间。若共享已有空间，则需要建立相应的链接
3. 初始化PCB：进程标识、处理机状态信息、进程状态
4. 建立链接：若调度队列是链表，则将新进程插入到就绪或就绪挂起链表中
5. 建立或扩展其他数据结构

##### Process Termination(终止)

- Batch job issues (问题) *Halt* instruction
- User logs off
- Quit an application
- Error and fault conditions (条件错误)

> Reasons for Process Termination
>
> - Normal completion
> - Time limit exceeded (超过)
> - Memory unavailable
> - Bounds (界限) violation (违反)
> - Protection error
> - Arithmetic error 计算错
> - Time overrun
> - I/O failure
> - Invalid (无效) instruction
> - Privileged instruction 特权指令
> - Data misuse (误用)
> - Operating system intervention (干涉) ， 操作员或OS干预，例如发生死锁
> - Parent terminates so child processes terminate
> - Parent request

##### destroy步骤

1. 根据被终止进程的标识符ID，找到其PCB，读出其进程的状态；
2. 若该进程为执行状态，则终止其执行，调度新的进程执行；
3. 若该进程有子孙进程，则立即终止其子孙进程的执行；
4. 将该进程的全部资源，或归还给其父进程，或归还给系统；
5. 将被终止进程的PCB从所在的队列中移除，等待其他程序来收集信息；

##### Process Block and Wakeup

- 请求系统服务
- 启动某种操作，例如：I/O
- 新数据尚未到达
- 无新工作可做

**阻塞原语BLock()**

​	当出现阻塞事件，进程通过调用阻塞原语来将自己阻塞。状态变为“阻塞状态”，并进入相应事件的阻塞队列

**唤醒原语wakeup()**

​	当阻塞进程期待的事件发生时，有关进程调用唤醒原语，将等待该事件的进程唤醒。状态变为ready，插入就绪队列

##### Process Suspend and Active

**挂起原语suspend()**

​	当出现挂起事件，系统利用挂起原语将制定进程或阻塞状态进程挂起。进程从内存到外存，状态改变

**激活原语active()**

​	当激活事件发生时，系统利用激活原语将制定进程激活。进程从外存换入内存，状态改变。