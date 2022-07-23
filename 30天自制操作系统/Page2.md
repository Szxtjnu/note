```assembly
;hello-os
;TAB=4
	ORG 0x7c00;

	JMP entry
	DB 0x90

entry:
	MOV AX,0
	MOV SS,AX
	MOV SP,0x7c00
	MOV DS,AX
	MOV ES,AX
	
	MOV SI,msg

putloop:
	MOV AL,[ST]
	ADD ST,1
	CMP AL,0
	
	JE fin
	MOV AH,0x0e
	MOV BX,15
	INT 0x10
	JMP putloop

fin:
	HLT
	JMP fin
	
msg:
	DB 0x0a, 0x0a
	DB "hello world"
	DB 0x0a
	DB 0
```

这段程序汇编程序中有许多新指令。

1. ORG,这个指令会告诉软件，在开始执行的时候，把这些机器语言指令装载到内存中的哪个地址。如果没有它，指令就不能被正确地翻译和执行。此外，有了这个地址，$的含义也随之变化，不再是指输出文件的第几个字节，而是代表将要读入的内存地址。

	ORG来源"origin"，源头，起点的意思。

2. JMP指令，这个指令相当于goto语句

3. “entry:”，这是标签的声明，用于制定JMP指令的跳转目的地等。

4. MOV，MOV指令应该是最常用的指令，它的含义是赋值，一般地用法是MOV AX,0 所代表的的意思就是将0的值赋值给AX

5. 上面的AX，CX都是CPU中寄存器的变量名

	> AX——accumulator，累加寄存器
	>
	> CX——counter ，计数寄存器
	>
	> DX——data，数据寄存器
	>
	> BX——base，基址寄存器
	>
	> SP——stack pointer， 栈指针寄存器
	>
	> BP——base pointer，基址指针寄存器
	>
	> ST——source index，源变址寄存器
	>
	> DT——destination index，目的变址寄存器

这些寄存器全都是16位寄存器，可以存放16位的二进制数。在平常使用的时候一般使用他们的缩写

