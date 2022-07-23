#### Process Description

？OS如何感知进程、控制进程及其所用的系统资源

##### Operating System Control Structures

- Information about the current (现在的) status of each process and resource
- Tables are constructed (构成) for each entity (实体) the operating system manages.
	- Memory Tables
	- I/O Tables
	- File Tables
	- Process Tables

##### Memory Tables

- Allocation (分配) of main memory to processes.

- Allocation of secondary memory (外存) to processes;

- Protection attributes (属性) for access to shared memory regions.

	访问内存控制区的保护属性，即进程对于内存空间的权限

- Information needed to manege virtual memory.

##### I/O Tables

- I/O device is available or assigned (已分配的).
- Status of I/O operation
- Location in main memory being used as the source or destination (终点) of the I/O transfer (传输).

##### File Tables 

- Existence (存在) of files.
- Location on secondary memory (外存)
- Current Status;
- Attributes;
- Sometimes this information is maintained (维护) by a file-management system.

##### Process Table

- Where process is located;
- Attributes necessary for its management
	- Process ID
	- Process state (状态)
	- Location in memory

#### Process Location

- Process includes set of programs to be executed (执行).
	- Data locations for local and global variables.
	- Any defined constants (常量).
	- Stack
- Process control block (PCB)
	- Collection of attributes.
- Process image (进程映像)
	- Collection of program, data, stack, and attributes.

##### Process image

- User Data
- User Program
- System Stack 存放系统以及过程调用地址、参数
- Process Control Block (PCB) OS感知进程、控制进程的数据结构

#### Process Control Block

- 简称PCB：是OS控制和管理进程时所用的基本数据结构
- 作用：PCB是相关进程存在于系统中的唯一标志；系统根据PCB而感知相关进程的存在
- 内容：通常情况下，PCB包含 Identifiers(标识)、状态、控制、指针等多种信息。

##### Process identification

- Identifiers
	- Identifier of this process
	- Identifier of the process that created this process(Parent Process)
	- User identifier

##### Process State Information

- User-Visible Registers (用户可见寄存器)

	- A user-visible register is one that may be referenced by means of the machine language that processor executes.
	- Typically, there are from 8 to 32 of these registrers.

- Control and Status Registers

	There are a varity of processor registers that are employed to control the operation of the processor. These include

	- Program counter: Contains the address of the next instruction to be fetched.
	- Condition codes: Result of the most recent arithmetic (计算) or logical (逻辑) operatinon (e.g. sign, zero, carry, equal, overflow)