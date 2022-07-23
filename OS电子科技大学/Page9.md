### Concurrency Deadlock And Starvation

> #### <u>*内容提要*</u>
>
> - 产生死锁与饥饿的原因
> - 解决死锁的方法
> - 死锁/同步的经典问题：哲学家进餐问题

#### Deadlock

- Permanent blocking (永久阻塞) of a set of processes that either compete for system resources or communicate with each other.
- No efficient solution.
- Involve (陷入) conflicting (冲突) needs for resources by two or more processes. 
	- 单进程死锁是存在的

##### Reusable Resources (可重用资源)

- Used by one process at a time and not depleted by that use.
- Processes obtain resources that they later release for reuse by other processes.
- Processors, I/O channels, main and secondary memory, files, databases, and semaphores.
- Deadlock occurs if each process holds one resource and requests the other.

###### Example of Deadlock

![](https://pic.imgdb.cn/item/625ce5c4239250f7c5746f7e.jpg)

##### Consumable Resources (可消耗资源)

- Created (produced) and destroyed (consumed) by a process.
- Interrupts, signals, messages, and information in I/O buffers.
- 可消耗资源也会存在出现死锁的可能，例如消息传递中的死锁现象的产生，此类死锁是由于设计失误造成的，很难发现，而且潜伏期比较长

##### Conditions for Deadlock

- Mutual exclusion
	- only one process may use a resource at a time
- Hold and wait (保持并等待)
	- A process may hold allocated resources while awaiting assignment of other resources.
- No preemption (不剥夺)
	- No resource can be forcibly removed from a process holding it.
- Circular wait (环路等待)
	- A closed chain of processes exists, such that each process holds at least one resource needed by the next process in the chain.

条件Mutual exclusion、Hold-and-wait、No preemption是死锁产生的必要条件，而非充分条件。条件Circular wait是前三个条件产生的结果。

##### Deadlock Prevention (预防死锁)

<u>间接方法</u>：禁止前3个条件之一的发生；

1. 互斥：是某些系统资源固有的属性，不能禁止
2. 禁止<u>保持并等待</u>条件：要求进程一次性地申请其所需要的全部资源。若系统中没有足够的资源可分配给它，则进程阻塞。
3. 禁止<u>不剥夺</u>条件：
	1. 若一个进程占用了某些系统资源，又申请新的资源，则不能立即分配给它。必须让它首先释放出已占有资源，然后再重新申请；
	2. 若一个进程申请的资源被另一个进程占有，OS可以剥夺低优先权进程的资源分配给高优先权的进程（要求此类可剥夺资源的状态易于保存和恢复，否则不能剥夺）

<u>直接方法</u>，禁止条件4（环路等待）的发生

​	即禁止“环路等待”条件：可以将系统的所有资源按类型不同进行线性排队，并赋予不同的序号。进程对某类资源的申请只能按照序号递增的方式进行。但显然这样的方法是低效的，它将影响进程执行的速度，甚至阻碍资源的正常分配。

##### Deadlock Avoidance (避免死锁)

<u>预防死锁</u>通过实施较强的限制条件实现，降低了系统性能。

<u>避免死锁</u>的关键在于为进程分配资源之前，首先通过计算， 判断此次分配是否会导致死锁，只有不会导致死锁的分配才可以执行。

A decision is made dynamically whether the current resource allocations request will, if granted, potentially lead to a deadlock.

Requires knowledge of future process request.

###### 2 Approaches to Deadlock Avoidance

- Do not start a process if its demands might lead to deadlock.
- Do not grant an incremental resource request to a process if this allocation might lead to deadlock.

##### Resource Allocation Denial

- Referred to as the banker's algorithm.
- State of the system is the current allocation of resources to process.
- Safe state is where there is at least one sequence that does not result in deadlock.
- Unsafe state is a state that is not safe.

###### Safe State

- 指系统能按某种顺序如<P1，P2，...，Pn>，来为每个进程分配其所需资源，直到最大需求，使每个进程都可以顺序完成，则称系统处于safe state。
- 若系统不存在这样的一个安全序列，则称该系统处于unsafe state.

##### Safe State v.s. Unsafe State

- 并非所有不安全状态都是死锁状态
- 当系统进入不安全状态后，便可能进入死锁状态
- 只要系统处于安全状态，则可以避免进入死锁状态

#### 银行家算法

- 该算法可用于银行发放一笔贷款之前，预测该笔贷款是否会引起银行资金的周转问题。
- 这里，银行的资金就类似于计算机系统中的资源，贷款业务就类似于计算机的资源分配。银行家算法能预测一笔业务对银行是否是安全的。该算法也能预测一次资源分配对计算机来说是否是安全的。
- 为了实现银行家算法，系统中必须设置若干的数据结构

##### 数据结构

1. 可以用资源向量Available：是一个具有m个元素的数组，其中的每个元素代表一类可利用资源的数目，其初始值为系统中该类资源的最大可用数目。其值将随着该类资源的分配与回收而动态改变。Available[j]=k，表示系统中现有Rj类资源k个。
2. 最大需求矩阵Max：是一个n*m的矩阵，定义了系统中n个进程中的每一个进程对m类资源的最大需求。Max(i,j) = k，表示进程i对Rj类资源的最大需求数目为k个。
3. 分配矩阵Allocation：是一个n*m的矩阵，定义了系统中每一类资源的数量。
4. 需求矩阵Need：是一个n*m 的矩阵，用来表示每一个进程尚需的各类资源数。

上述三类矩阵存在下述关系：
$$
Need[i,j]=Max[i,j]-Allocation[i,j]
$$

##### 银行家算法的安全性算法

1. 设置两个工作向量：

	1. 设置一个数组Finish[n]。当Finish[i]=True时，表示进程Pi可以获得其所需的全部资源，而顺利执行完成。
	2. 设置一个临时向量Work，表示系统可以提供给进程继续运行的资源的集合。安全性算法刚开始执行的时候，Work = Available。

2. 从进程集合中找到一个满足下列条件的进程：

	Finish[i] = false；并且Need≤Work；

	若找到满足条件的进程则执行步骤3，若未找到则执行步骤4

3. 当进程Pi获得全部资源后，将顺利执行直至完成，并释放所拥有的全部资源，故执行以下操作：

	Work = Work + Allocation(i)；以及 Finish[i] = true;

	转向步骤2

4. 如果所有进程的Finish[i]=TRUE，则表示系统处于安全状态，则系统则处于不安全状态。

#### Deadlock Avoidance

- Maximum resource requirement must be stated in advance
- Processes under consideration must be independent; no synchronization requirements.
- There must be a fixed number of resources to allocate.
- No process may exit while holding resources.

#### Deadlock Detecetion

- 检测死锁不同于预防死锁，不限制资源访问方式和资源申请
- OS周期性地执行死锁检测例程，检测系统中是否出现“环路等待”

##### Stategies once Deadlock Detected

- Abort (终止) all deadlock processes.
- Back up each deadlock process to some previouly defined checkpoint, and restart all process.
	- Original deadlock may occur.
- Successively abort deadlock processes until deadlock no longer exists.
- Successively preempt resouces until deadlock no longer exists.

##### Selection Critreia (选择标准) Deadlocked Processes

- Least amount of processor time consumed so far.
- Least number of lines of output produced so far;
- Most estimated time remaing;
- Least totoal resources allocated so far;
- Lowest priority;

#### Dining Philosophers Problem

- 描述：有5个哲学家，他们的生活方式是交替地进行思考和进餐。哲学家们共用一张圆桌，分别坐在周围的五把椅子上。圆桌中间放有一大碗面条，每个哲学家分别有一个盘子和一个叉子，如果哲学家想吃面条，则必须拿到靠其最近的左右两支叉子。进餐完毕，放下叉子继续思考。
- 要求：设计一个合理的算法，使全部哲学家都能进餐（非同时）。算法必须避免死锁或饥饿，哲学家互斥共享叉子。
