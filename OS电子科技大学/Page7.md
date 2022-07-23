### Real - Time Scheduling

Correctness (正确性) of the system depends not only on the logical result of the computation but also on the time at which the results are produced.

<u>Tasks or processes</u> attempt to control or react to events that take place in the outside world.

These events occur in "real time" and process must be able to keep with them.

#### Real - Time Systems

- Control of laboratory experiments
- Process control plants
- Robotics
- Air traffic control
- Telecommunications
- Military command and control systems

指能及时响应外部事件的请求，在规定的时间内完成对该事件的处理，并控制所有实时任务协调一致地运行的计算机系统。

实时系统分为实时控制系统和实时信息处理系统。

#### Real - Time Task

- 按任务执行是否呈现周期性来划分
	- Periodic (周期性) 实时任务
	- Aperiodic (非周期性) 实时任务，必须联系中一个deadline
- 根据对截止时间的要求来划分
	- hard real-time task (硬实时任务)，系统必须满足任务对截止时间的要求，否则可能出现难以预测的结果
	- soft real - time task (软实时任务) 

#### Characteristics of Real - Time Operating Systems

- Deterministic (确定性)
	- Operations are performed at fixed, predetermined times or within predetermined time intervals (确定的时间段) .
	- Concerned with how long the operating system delays before acknowledging an interrupt.
- Responsiveness (响应性)
	- How long, after acknowledgment, the operating system to service the interrupt.
	- Includes amount of time to begin execution of the interrupt.
	- Includes the amount of time to perform the interrupt.

#### Characteristics of Real - Time Operating Systems

- User control
	- User specifies priority (优先)
	- Specify paging (存储分页)
	- What processes must always reside in main memory
	- Disks algorithms to use
	- Right of processes

#### Features of Real - Time Operating Systems 

- Fast context  switch
- Small size
- Ability to respond to external interrupts quickly.
- Multitasking with interprocess communication tools such as semaphores, signals and events.
- Files that accumulate data at a fast rate.
- Use of special sequential files that can accumulate data at a fast rate.
- Preemptive scheduling base on priority
- Minimization (最小限度) of intervals during which interrupts are disabled.
- Delay tasks for fixed amount of time.
- Special alarms and timeouts.



#### Scheduling of a Real - Time Process

- Round Robin Preemptive Scheduler (基于时间片的轮转调度法)
- Priority - driven Nonpreemptive Scheduler (基于优先级的非剥夺调度法)
- Priority - driven Preemptive Scheduler (基于优先级的剥夺调度法)
- Immediate Preemptive Scheduler (立即剥夺调度法)

##### Round Robin Preemptive Scheduler

<img src="https://pic.imgdb.cn/item/62551405239250f7c541d346.jpg" style="zoom: 80%;" />

响应时间在<u>秒级</u>

广泛应用于分时系统，也可用于一般的实时信息处理系统

不适用于要求严格的实时控制系统

##### Priority- driven Nonpreemptive Scheduler

为实时任务赋予较高的优先级，将它插入到就绪队列的队首，只要正在执行的进程释放Processor, 则立即调度该实时任务执行

![](https://pic.imgdb.cn/item/625514d2239250f7c542e914.jpg)

响应时间在数百毫秒至数秒范围

大多应用于多道批处理系统，也可以运用于要求不太严格的实时操作系统

##### Priority- driven preemptive Scheduler

![](https://pic.imgdb.cn/item/62552128239250f7c554ef15.jpg)

当实时任务到达后，可以在时钟中断时，剥夺正在执行的低优先级进程的执行，调度执行高优先级的任务

响应时间较短，一般在几十毫秒或几毫秒

##### Immediate Preemptive Scheduler

![](https://pic.imgdb.cn/item/625521ed239250f7c55625f5.jpg)

要求操作系统具有快速响应外部事件的能力。一旦出现外部中断，只要当前任务未处于临界区，便立即剥夺其执行，把处理机分配给请求中断的紧迫任务

调度时延可以降至100微妙，甚至更低

#### Real - Time Scheduling

##### Aim of Real-Time scheduling

Hard real-time task, 在其规定的截止时间内完成

尽可能使Soft real-time task 也能在规定的截止时间内完成

公平性和最短平均响应时间等要求已不再重要。但是，大多数现代操作系统无法直接处理任务的截止时间，他们只能尽量提高响应速度，以尽快地调度任务。

##### Static table-driven approaches

- 用于调度周期性实时任务
- 按照任务周期到达的时间、执行时间、完成截止时间以及任务的优先级，制定调度表，调度实时任务
- 最早截止时间优先(EDF)调度算法即属于此类
- 此类算法不灵活，任何任务的调度申请改动都会引起调度表的修改

##### Static priority-driven preemptive approaches

- 此类算法多用于非实时多道程序系统
- 优先级的确定方法很多，例如在分时系统中， 可以对I/O bound 和Processor bound 的进程赋予不同的优先级
- 实时系统中一般根据对任务的限定时间赋予优先级，例如速度单调算法(RM)即使为实时任务赋予静态优先级

##### Dynamic planning-based approaches

- 当实时任务到达之后，系统为新到达的任务和正在执行的任务动态创建一张调度表
- 在当前执行进程表不会错过其截止时间的条件下，如果也能使新到达任务在截止时间内完成，即立刻调度执行新任务

##### Dynamic best effort approaches

- 实现简单，广泛用于非周期实时任务调度
	- 当任务到达时，系统根据其属性赋予优先级，优先级高的先调度。例如最早截止时间优先EDF调度算法就采用了这种方法。这种算法总是尽最大努力尽早调度紧迫任务，因此成为“最大努力调度算法”
	- 缺点在于，当任务完成，或截止时间到达时，很难知道该任务是否满足其约束时间

#### Deadline Scheduling

##### Information used

- Ready time
- Starting deadline
- Completion deadline
- Processing time
- Resource requirements
- Priority
- Subtask scheduler

> Which task to schedule next?
>
> ​	Scheduling tasks with the earliest deadline minimized the fraction of tasks that miss their deadlines.
>
> What sort of preemption is allowed?
>
> ​	When starting deadlines are specified, then a nonpreemtive scheduler makes sense.在执行完强制子任务或临界区后，阻塞自己。
>
> ​	For a system with completion deadlines, a preemptive strategy is most appropriate.

##### Earliest Deadline (最早截止时间优先，简称ED)

- 常用调度算法
- 若制定任务的Starting deadlines, 则采用 Nonpreemption, 当某任务的开始截止时间到达时，正在执行的任务必须执行完其强制部分或临界区，释放CPU，调度开始截止时间到的任务执行
- 若制定任务的Completion deadlines, 则采用 Preemption

##### Periodic tasks with completion deadlines 

- 由于此类任务是周期性的、可预测的，可采用静态表驱动之最早截止时间优先调度算法，使系统中的任务都能按要求完成
- 举例：
	- 周期性任务A和B，制定了它们的完成截止时间，任务A每隔20ms完成一次，任务B每隔50毫秒完成一次。任务A每次需要执行10ms，任务B每次需要执行25ms

##### Aperiodic (非周期) tasks with starting deadlines

- 可以采用<u>最早截止时间优先调度算法</u>或允许CPU空闲的EDF算法
- Earliest Deadlines with Unforced Idle Times, 指优先调度最早截止时间的任务，并将它执行完毕才调度下一个任务。即使选定的任务未就绪，允许CPU空闲等待，也不能调度其他任务。尽管CPU的利用率不高，但这种调度算法可以保证系统中的其他任务都能按要求完成。

#### Rate Monotonic (单调) Scheduling

Assigns priorities to tasks on the basis of their periods.

Highest-priority task is the one with the shortest period.

Period (任务周期)，指一个任务到达至下一任务到达之间的时间范围

Rate (任务速度)，即周期的倒数，以Hz为单位

任务周期的结束，表示任务的硬截止时间，任务的执行时间不应超过任务周期

CPU的利用率 = 任务执行时间 / 任务周期

在RMS调度算法中，如果以任务速度为参数，则优先级函数是一个单调递增的函数

##### Periodic Task Timing Diagram 

![](https://pic.imgdb.cn/item/625791cc239250f7c56946dd.jpg)

![](https://pic.imgdb.cn/item/625791d8239250f7c5695977.jpg)