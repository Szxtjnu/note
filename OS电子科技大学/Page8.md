### Concurrency : Mutual Exclusion and Synchronization

#### Design Issues of Concurrency

- Communication among processes
- Sharing/Competing of resources
- Synchronization of multiple processes
- Allocation of processor time

#### Difficulties with Concurrency

- Sharing global resources
- Management of allocation of resources
- Programming errors difficult to locate

#### A Simple Example

```
void echo()
{	
	chin = getchar();
	chout = chin;
	putchar(chout);
}
```

如果是并发执行的进程？

```
Process P1
in = getchar();
...
chout = in;
...
putchar(chout);
```

```
Process P2
in = getchar();
...
chout = in;
...
putchar(chout);
```

P1和P2中的in和out都是全局变量，当P1&&P2同时执行的时候，会发生并发的状态，那么最终输出的结果是P1中的in还是P2中的in是无法确定的，这里的变量没有保护，那么如何才能设计一个确定的模式？

#### Operating System Concerns

- Keep track of active processes: PCB 

- Allocate and deallocate resources 分配与回收资源
	- Processor time: scheduling
	- Memory: virtual memory
	- Files
	- I/O devices
- Protect data and resources
- Result of process must be independent of the speed of execution of other concurrent processes.

#### Process Interaction (相互作用)

- Process unaware of each other
	- Competition (竞争)
	- Mutual exclusion, Deadlock, Starvation
- Processes indirectly aware of each other
	- Cooperation by sharing
	- Mutual exclusion, Deadlock, Starvation, Data coherence (一致性)
- Process directly aware of each other
	- Cooperation by communication
	- Deadlock, Starvation

##### Competition Among Processes for Resources

- Mutual Exclusion (互斥)

	- Critical sections (临界区)
		- Only one program at a time is allowed in its critical section.
		- Eg. Only one process at a time is allowed to send command to the printer (critical resource) .

- Deadlock (死锁)

	- 例如，有两个进程P1，P2，竞争两个资源A、B。

	- 假设：

		*<u>占用</u>*：P1(B) and P2(A)

		*<u>申请</u>*：P1(A) and P2(B)

		结果就会导致P1和P2永久等待，即死锁状态。

	<img src="https://pic.imgdb.cn/item/6257cfc8239250f7c5c8bfad.jpg" alt="DeadLock" style="zoom:50%;" />

- Starvation

> ##### Mutual Exclusion Mechanism (互斥机制)
>
> entry section -> critical section -> exit section

##### Cooperation Among Processes by Sharing

- Writing must be mutually exclusive (写必须互斥)
- Critical sections are used to provide data integrity (数据完整性) . 

##### Cooperation Among Processes by Communication

- Messages are passed
	- Mutual exclusion is not a control requirement
- Possible to have deadlock
	- Each process waiting for a message from the other process.
- Possible to have starvation
	- Two processes sending message to each other while another process waits for a message.

#### Requirements for Mutual Exclusion

- Only one process at a time is allowed in the critical section for a resource
- A process that halts (停止) in its non-critical section must do so without interfering with other processes.
- No deadlock or starvation.
- A process must bot be delayed access to a critical section when there is no other process using it.
- No assumptions (假定) are made about relative process speeds or number of processes.
- A process remains inside its critical section for a finite (限定的) time only.
- **空闲让进、忙则等待、有限等待、让权等待**

#### Approaches of Mutual Exclusion

##### Software Approaches

- Memory access level
- Access to the same location in main memory are serialized by some sort of memory arbiter (仲裁者). 
- Dekker's Algorithm
- Peterson's Algorithm
- To mutual exclusion for two processes.
- To illustrate most of the common bugs encountered in developing concurrent programs.
- Is likely to have high processing overhead.
- The risk of logical errors is significant.

##### Hardware Support

- Interrupt Disabling (屏蔽中断)
	- A process runs until it invokes an operating-system service or until it is interrupted.
	- Disabling interrupts guarantees (保证) mutual exclusion.
- The price of this approach is high.
- Multiprocessing
	- Disabling interrupts on one processor will not guarantee mutual exclusion.

> ###### Mutual Exclusion by Interrupt Disabling
>
> Disable interrupts -> critical section -> Enable interrupts

- Special Machine Instructions

	- Performed in a single instruction cycle.
	- Not subject to interference from other instructions (避免冲突).

- Test and Set Instruction

	```pascal
	function testset ( var i:integer):boolean;
		begin
		if i=0 then
			begin
			i := 1;
			testset := true;
		end
		else testset := false;
		end
	```

	```pascal
	begin /*主程序*/
		bolt := 0;
		parbegin
		P(1);
		P(2);
		...
		P(n);
		parend
	end
	
	program mutualexclusion;
	const n = ...;/*进程数*/
	var bolt:integer;
	procedure P(i:integer);
	begin
		repeat
		repeat {nothing} until testset (bolt);
		<临界区>;
		bolt = 0;
		<其余部分>
		forever
	end;
	```

- Advantages

	- Applicable to (适应) any number of processes on either a single processor or multiple processors sharing main memory.
	- It is simple and therefore easy to verify.
	- It can be used to support multiple critical sections.

- Disadvantages

	- Busy-waiting is employed.
	- Starvation is possible.
		- When a process leaves a critical section and more than one process is waiting.
	- Deadlock is possible.
		- If a low priority (优先级) process has the critical region and a higher priority process needs, the higher priority process process will obtain (获得) the processor to wait for the critical region.

#### Semaphores

- Special variable called a semaphore is used for signaling.
- If a process is waiting for a signal, it is blocked until that signal is sent.
- Wait and Signal operations cannot be interrupted.
- Queue is used to hold processes waiting on the semaphore.
- Semaphore is a variable that has an integer value.
	- May be initialized to a nonnegative (非负) number.
	- Wait and Signal are primitives (atomic, cannot be interrupted and each routine can be treated as an indivisible step).
- Wait operation decrements (递减) the semaphore value.
	- wait(s) : s - 1
	- wait 操作：申请资源且可能阻塞自己(s<0)
- Signal operation increments semaphore value
	- signal(s) : s + 1
	- signal操作：释放资源并唤醒阻塞进程(s<=0)

##### Types of Semaphores

- General Semaphore (通用信号量)
  - 通用信号量是记录型，其中一个域为整型，另一个域为队列，其元素为等待该信号量的阻塞进程（FIFO)
- Binary Semaphore(二进制信号量)

##### General Semaphore

```pascal
type semaphore = record
count : integer;
queue : list of process
end;
var s: semaphore;
```

##### wait/signal Operations

```pascal
wait(s);
s.count := s.count-1;
if s.count<0
	then begin
	block P;
	insert P into s.queue;
end;
```

```pascal
signal(s):
s.count := s.count+1;
if s.count<=0
	then begin
	wakeup the first P;
remove the P from s.queue;
end;
```

```pascal
program mutualexclusion;
const n =...;//进程数
var s:sqmaphore(:=1);//定义信号量s,s.count初始化为1
procedure P(i:integer);
begin
	repeat
		wait(s);
		<临界区>;
		signal(s);
		<其余部分>;
	foerver
end;

begin //主程序
parbegin
P(1);P(2);...;P(n);
parend;
end;
```

##### 信号量的类型

- 信号量分为：互斥信号量和资源信号量
	- 互斥信号量用于申请或释放资源的使用权，常初始化为1
	- 资源信号量用于申请或归还资源，可以初始化为大于1的正整数，表示系统中某类资源的可用个数
- wait操作用于申请资源（或使用权），进程执行wait原语时，可能会阻塞自己
- signal操作用于释放资源（或归还资源使用权），进程执行signal原语时，有责任唤醒一个阻塞进程

#####  信号量的意义

1. <u>互斥信号量</u>：申请/释放使用权，常初始化为1
2. <u>资源信号量</u>：申请/归还资源，资源信号量可以初始化为一个正整数（表示系统中的某类资源的可用个数），s.count的意义为：
	1. s.count≥0：表示还可以执行wait(s)而不会阻塞的进程数（即可用资源数）；
	2. s.count<0：表示s.queue队列中阻塞进程的个数（即被阻塞进程数）；

> s.count的取值范围
>
> 当仅有两个并发进程共享临界资源时，互斥信号量仅能取值0、1、-1.其中：
>
> s.count = 1，表示无进程进入临界区
>
> s.count = 0，表示已经有一个进程进入临界区
>
> s.count = -1，则表示已有一进程正在等待进入临界区
>
> 当用s来实现n个进程的互斥时，s.count的取值范围是1~-(n-1)

- 操作系统内核以系统调用形式提供wait和signal原语，应用程序通过对系统调用实现进程间的互斥。
- 工程实践证明，利用信号量方法实现进程互斥时高效的、一直被广泛采用

> P(S)是wait(S)，V(S)是signal(S)

#### Producer/Consumer Problem

- One or more producers are generating data and placing data （产生数据并存放）in a buffer.
- A single consumer is taking items out of the buffer one at time.
- Only one producer or consumer may access the buffer at any one time.

- 任务以及要求
	1. Buffer 不能并行操作（互斥），即某时刻只允许一个实体（producer or consumer）访问Buffer；
	2. 控制producer and consumer同步读/写buffer，即不能向满buffer写数据也不能从空buffer中读数据

> 如果不控制生产者和消费者？
>
> 指针in和out初始化指向缓冲区的第一个存储单元。
>
> 生产者通过in指针向存储单元存放数据，一次存放一条数据，且in指针向后移一个位置。
>
> 消费者从缓冲区中逐条取走数据，一次取一条数据，响应的存储单元变为“空”。每取走一条数据，out指针向后移一个存储单元位置。

##### 生产者/消费者必须互斥

- 生产者和消费者可能同时进入缓冲区，甚至可能同时读/写一个存储单元，将导致执行结果不确定。
- 这显然是不允许的。必须使生产者和消费者<u>互斥</u>进入缓冲区。即，某时刻只允许一个实体（生产者或消费者）访问缓冲区，生产者互斥消费者和其他的任何生产者。
- 生产者不能向满缓冲区写数据，消费者也不能在空缓冲区中取数据，即生产者与消费者必须<u>同步</u>

![](https://pic.imgdb.cn/item/625bc618239250f7c5b0e5b9.jpg)

```pascal
program producer consumer;
const sizeofbuffer=...;//缓冲区大小
var s:semaphore(:=1);//互斥信号量，初始化为1
var n:semaphore(:=0);//资源信号量，数据单元，初始化为0
var e:semaphore(:=sizeofbuffer);//资源信号量，空存储单元
procedure producer;
begin
	repeat
	生产一条数据;
	wait(e);
	wait(s);
	存入一条数据;
	signal(s);
	signal(n);
	forever
end;
procedure consumer;
begin
	repeat
	wait(n);
	wait(s);
	取一条数据;
	signal(s);
	signal(e);
	消费数据;
	forever
end;

begin//主程序
parbegin
producer;consumer;
parend
end;
```

> P.S.
>
> 1. 进程应该先申请资源信号量，再申请互斥信号量，顺序不能颠倒
> 2. 对任何信号量的wait和signal操作必须配对。同一进程中的多对wait和signal语句只能嵌套，不能交叉
> 3. 对同一个信号量的wait和signal可以不在同一个进程中
> 4. wait和signal语句不能颠倒顺序，wait语句一定在于signal语句

#### Readers/Writers Problem

- Any number of readers may simultaneously (同时) read the file.
- Only one writers at a time may write to the file.
- If a writer is writing to the file, no reader may read it.

可用于解决多个进程共享一个数据区（文件、内存区、一组寄存器等），其中若干读进程只能读数据，若干写进程只能写数据等实际问题

##### Readers have priority

指一旦有读者正在读数据，允许多个读者同时进入读数据，只有当全部读者退出，才允许写者进入写数据

PS: Writers are subject to starvation.

```pascal
program readers_writers;
const readcount:integer;
var x, wsem:semaphore(:=1);
procedure reader;
begin
	repeat
	wait(x);
	readcount:=readcount + 1;
	if readcount = 1 then wait(wsem);
	signal(x);
	读数据;
	wait(x);
	readcount:= readcount - 1;
	if readcount = 0 then signal(wsem);
	signal(x);
	forever
end;
procedure writer;
begin
	repeat
	wait(wsem);
	写数据;
	signal(wsem);
	forever
end;

begin
readcount:=0;
parbegin
reader;writer;
parend;
end;
```

##### Readers/Writers Problem

- Writers have priority：指只要有一个writer申请写数据，则不再允许新的reader进入读数据

```pascal
program readers_writers;
const readcount, writecount:integer;
var x, y, z, rsem, wsem:semaphore(:=1);
procedure reader;
begin
	repeat
	wait(z);
	wait(rsem);
	wait(x);
	readcount:=readcount + 1;
	if readcount = 1 then wait(wsem);
	signal(x);
	signal(rsem);
	signal(z);
	读数据;
	wait(x);
	readcount:=readcount - 1;
	if readcount = 0 then signal(wsem);
	signal(x);
	forever
end;

procedure writer;
begin
	repeat
	wait(y);
	writecount:=writercount + 1;
	if writecount = 1 then wait(rsem);
	signal(y);
	wait(wsem);
	写数据;
	signal(wsem);
	wait(y);
	writecount:=writecount - 1;
	if writecount = 0 then signal(rsem);
	signal(y);
	forever
end;
begin
	readcount:=0;writecount:=0;
	parbegin
	reader;writer;
	parend
end;
```

#### Monitors (管程)

- 用信号量实现互斥，编程容易出错(wait 和signal 的出现顺序和位置非常重要)
- Support at Programming-language level
- 管程是用并发Pascal等语言编写的程序，现在已经形成了许多库函数。管程可以锁定任何对象，如链表或链表的元素
- 用管程实现互斥比用信号量实现互斥，更简单、方便。
- Monitor is a software module, 由若干过程、局部于管程的数据、初始化语句（组）组成
- Chief characteristics
	- Local data variables are accessible only by the monitor
	- Process enters monitor by invloking (唤醒) one of ite procedures.
	- Only one process may be executing in the monitor at a time.

#### Message Passing

- Enforce mutual exclusion

- Exchange information

	send (destination, message)

	receive (source, message)

##### Synchronization (同步)

- Sender and receiver may or may not be blocked (waiting for message)
- Blocking send, blocking receive
	- Both sender and receiver are blocked until message is delivered.
	- Called a rendezvous (紧密同步，汇合).
- Nonblocking send, blocking receive
	- Sender continues processing such as sending messages as quickly as possible.
	- Receiver is blocked until the requested message arrives.
- Nonblocking send, nonblocking receive
	- Neither party is required to wait.

##### Addressing

- Direct addressing 
	- Send primitive includes a specific indentifier of the destination process.
	- Receive primitive could know ahead of time which process a message is expected.
	- Receive primitive could use soure parameter to return a value when the receive operation has been performed.
- Indirect addressing
	- messages are sent to a shared data strucutre consisiting of queues.
	- queues are called mailboxes.
	- one process sends a message to the mailbox and the other process picks up the message from the mailbox.

##### Message Passing (Mutual Exclusion)

- 若采用Nonblocking send, blocking receive
- 多个进程共享邮箱mutex。若进程申请进入临界区，首先申请从mutex邮箱中接受一条消息。若邮箱空，则该进程阻塞；

##### Message Passing

- 解决有限buffer Producer/ Consumer Problem
- 设两个邮箱：
	- May consume: Producer存放数据，供Consumer取走
	- May produce：存放空消息的buffer空间

#### Summary of Mutual Exclusion

1. 利用wait、signal原语对Semaphore互斥操作实现，powerful and flexible
2. 可用软件方法实现互斥，但是会增加处理负荷
3. 可用硬件或固件的方法实现互斥，如屏蔽中断、Test and Set指令等，会出现忙等