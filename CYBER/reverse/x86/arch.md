# registry:

Instruction Pointer : 
EAX or RAX(for 64bit): Results of arithmetic operations are often stored in this register
EBX or RBX(for 64 bit): Base address for referencing an offset.
ECX or RCX(64 bit): Counter Register and is often used in counting operations such as loops
EDX or RDX(64bit): This register is also called the Data Register
ESP or RSP(64 bit): it points to the top of the stack and is used in conjunction with the Stack Segment register
EBP or RBP(64 bit):Base Pointer. it is used to access parameters passed by the stack.
ESI or RSI(64 bit):Source Index register. It is used for string operations. It is used with the Data Segment (DS) register as an offset.
EDI or RDI(64 bit):Destination Index register. It is also used for string operations. It is used with the Extra Segment (ES) register as an offset.
R8-R15 (only in 64 bit system): used for general purpose
## Status Flag Registers: 
a register called EFLAGS in 32 bit or RFLAGS in 64. obly read byte not ine data
Zero Flag: ZF, the Zero Flag indicates when the result of the last executed instruction was zero. 
Carry Flag:Carry Flag indicates when the last executed instruction resulted in a number too big or too small for the destination
Sign Flag: SF indicates if the result of an operation is negative or the most significant bit is set to 1.
Trap Flag or TF indicates if the processor is in debugging mode. the CPU will execute one instruction at a time for debugging purposes.

# Segment

Code Segment: The Code Segment (CS ) register points to the Code section in the memory.

Data Segment: The Data Segment (DS) register points to the program's data section in the memory.

Stack Segment: The Stack Segment (SS) register points to the program's Stack in the memory.

Extra Segments (ES, FS, and GS): These extra segment registers point to different data sections. These and the DS register divide the program's memory into four distinct data sections. 

# Memory 


/// in the memory there are in order:
STACK
HEAP
CODE
DATA

## Code 

contains the code data

## Data 
The Data section contains initialized data that is not variable and remains constant.

## Stack

This section of the Memory contains local variables, arguments passed on to the program, and the return address of the parent process that called the program

## Heap
contains variables and data created and destroyed during program execution. When a variable is created, memory is allocated for that variable at runtime


#  Stack

for each function the stack go down and add a layer .
the address go down ! 

when a function is called we push the following data from top top down

Arg 1
Arg 2
Return Addres
SAVED EBP # the LAST EBP VALUE, the EBP register point here that ALLOW to know where are the return address and argument are top, seperate local ar from other store the last 
Local Variable
Local Variable
Local variable ... # THE ESP POINT TO THE LAST VARIABLE + one data 

