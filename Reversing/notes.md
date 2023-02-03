# **Reverse Engineering**

## **Notes**:

General-Purpose Registers (GPR) - 16-bit naming conventions
The 8 GPRs are as follows:

- Accumulator register (AX). Used in arithmetic operations
- Counter register (CX). Used in shift/rotate instructions and loops.
- Data register (DX). Used in arithmetic operations and I/O operations.
- Base register (BX). Used as a pointer to data (located in segment register DS, when in segmented mode).
- Stack Pointer register (SP). Pointer to the top of the stack.
- Stack Base Pointer register (BP). Used to point to the base of the stack.
- Source Index register (SI). Used as a pointer to a source in stream operations.
- Destination Index register (DI). Used as a pointer to a destination in stream operations.

In 16-bit mode, the register is identified by its two-letter abbreviation from the list above. In 32-bit mode, this two-letter abbreviation is prefixed with an 'E' (extended).

In the 64-bit version, the 'E' is replaced with an 'R' (register)

It is also possible to address the first four registers (AX, CX, DX and BX) in their size of 16-bit as two 8-bit halves. The least significant byte (LSB), or low half, is identified by replacing the 'X' with an 'L'. The most significant byte (MSB), or high half, uses an 'H' instead. For example, CL is the LSB of the counter register, whereas CH is its MSB.

![image](assets/registers.png)

![image](assets/identifiers.png)

<br>

### **EFLAGS Register**

The EFLAGS is a 32-bit register used as a collection of bits representing Boolean values to store the results of operations and the state of the processor.

![image](assets/eflags.png)

0.	CF : Carry Flag. Set if the last arithmetic operation carried (addition) or borrowed (subtraction) a bit beyond the size of the register. This is then checked when the operation is followed with an add-with-carry or subtract-with-borrow to deal with values too large for just one register to contain.
2.	PF : Parity Flag. Set if the number of set bits in the least significant byte is a multiple of 2.
4.	AF : Adjust Flag. Carry of Binary Code Decimal (BCD) numbers arithmetic operations.
6.	ZF : Zero Flag. Set if the result of an operation is Zero (0).
7.	SF : Sign Flag. Set if the result of an operation is negative.
8.	TF : Trap Flag. Set if step by step debugging.
9.	IF : Interruption Flag. Set if interrupts are enabled.
10.	DF : Direction Flag. Stream direction. If set, string operations will decrement their pointer rather than incrementing it, reading memory backwards.
11.	OF : Overflow Flag. Set if signed arithmetic operations result in a value too large for the register to contain.
12-13.	IOPL : I/O Privilege Level field (2 bits). I/O Privilege Level of the current process.
14.	NT : Nested Task flag. Controls chaining of interrupts. Set if the current process is linked to the next process.
16.	RF : Resume Flag. Response to debug exceptions.
17.	VM : Virtual-8086 Mode. Set if in 8086 compatibility mode.
18.	AC : Alignment Check. Set if alignment checking of memory references is done.
19.	VIF : Virtual Interrupt Flag. Virtual image of IF.
20.	VIP : Virtual Interrupt Pending flag. Set if an interrupt is pending.
21.	ID : Identification Flag. Support for CPUID instruction if can be set.

### **Instruction Pointer**

The EIP register contains the address of the next instruction to be executed if no branching is done.

EIP can only be read through the stack after a call instruction.


## Ref:
- https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture
- https://cs.brown.edu/courses/cs033/docs/guides/x64_cheatsheet.pdf

<br>

# **GDB**

You can examine the contents of memory using the `x/<n><u><f> <address>` parameterized command. In this format `<u>` is
the unit size to display, `<f>` is the format to display it in, and `<n>` is the number of elements to display. Valid
unit sizes are `b` (1 byte), `h` (2 bytes), `w` (4 bytes), and `g` (8 bytes). Valid formats are `d` (decimal), `x`
(hexadecimal), `s` (string) and `i` (instruction). The address can be specified using a register name, symbol name, or
absolute address. Additionally, you can supply mathematical expressions when specifying the address.

For example, `x/8i $rip` will print the next 8 instructions from the current instruction pointer. `x/16i main` will
print the first 16 instructions of main. You can also use `disassemble main`, or `disas main` for short, to print all of
the instructions of main. Alternatively, `x/16gx $rsp` will print the first 16 values on the stack. `x/gx $rbp-0x32`
will print the local variable stored there on the stack.

## Creating and manipulating stack with asm

**mov edx, 0FFFFFFFFh**

; --> this will be the start address of your stack, furthest away from your code and data, it will also serve as that register that keeps track of the last object in the stack that i explained earlier. You call it the "stack pointer", so you choose the register EDX to be what ESP is normally used for.

**sub edx, 4**

**mov [edx], dword ptr [someVar]**

; --> these two instructions will decrement your stack pointer by 4 memory locations and copy the 4 bytes starting at [someVar] memory location to the memory location that EDX now points to, just like a PUSH instruction decrements the ESP, only here you did it manually and you used EDX. So the PUSH instruction is basically just a shorter opcode that actually does this with ESP.

**mov eax, dword ptr [edx]**

**add edx, 4**

; --> and here we do the opposite, we first copy the 4 bytes starting at the memory location that EDX now points to into the register EAX (arbitrarily chosen here, we could have copied it anywhere we wanted). And then we increment our stack pointer EDX by 4 memory locations. This is what the POP instruction does.

[Ref](https://stackoverflow.com/questions/556714/how-does-the-stack-work-in-assembly-language)

## Stack vs. Heap

The stack is the memory set aside as scratch space for a thread of execution. When a function is called, a block is reserved on the top of the stack for local variables and some bookkeeping data. When that function returns, the block becomes unused and can be used the next time a function is called. The stack is always reserved in a LIFO (last in first out) order; the most recently reserved block is always the next block to be freed. This makes it really simple to keep track of the stack; freeing a block from the stack is nothing more than adjusting one pointer.

The heap is memory set aside for dynamic allocation. Unlike the stack, there's no enforced pattern to the allocation and deallocation of blocks from the heap; you can allocate a block at any time and free it at any time. This makes it much more complex to keep track of which parts of the heap are allocated or free at any given time; there are many custom heap allocators available to tune heap performance for different usage patterns.

Each thread gets a stack, while there's typically only one heap for the application (although it isn't uncommon to have multiple heaps for different types of allocation).

To answer your questions directly:

To what extent are they controlled by the OS or language runtime?

The OS allocates the stack for each system-level thread when the thread is created. Typically the OS is called by the language runtime to allocate the heap for the application.

**What is their scope?**

The stack is attached to a thread, so when the thread exits the stack is reclaimed. The heap is typically allocated at application startup by the runtime, and is reclaimed when the application (technically process) exits.

**What determines the size of each of them?**

The size of the stack is set when a thread is created. The size of the heap is set on application startup, but can grow as space is needed (the allocator requests more memory from the operating system).

**What makes one faster?**

The stack is faster because the access pattern makes it trivial to allocate and deallocate memory from it (a pointer/integer is simply incremented or decremented), while the heap has much more complex bookkeeping involved in an allocation or deallocation. Also, each byte in the stack tends to be reused very frequently which means it tends to be mapped to the processor's cache, making it very fast. Another performance hit for the heap is that the heap, being mostly a global resource, typically has to be multi-threading safe, i.e. each allocation and deallocation needs to be - typically - synchronized with "all" other heap accesses in the program.
___

### **Stack:**

- Stored in computer RAM just like the heap.
- Variables created on the stack will go out of scope and are automatically deallocated.
- Much faster to allocate in comparison to variables on the heap.
- Implemented with an actual stack data structure.
- Stores local data, return addresses, used for parameter passing.
- Can have a stack overflow when too much of the stack is used (mostly from infinite or too deep recursion, very large allocations).
- Data created on the stack can be used without pointers.
- You would use the stack if you know exactly how much data you need to allocate before compile time and it is not too big.
- Usually has a maximum size already determined when your program starts.

### **Heap:**

- Stored in computer RAM just like the stack.
- In C++, variables on the heap must be destroyed manually and never fall out of scope. The data is freed with delete, delete[], or free.
- Slower to allocate in comparison to variables on the stack.
- Used on demand to allocate a block of data for use by the program.
- Can have fragmentation when there are a lot of allocations and deallocations.
- In C++ or C, data created on the heap will be pointed to by pointers and allocated with new or malloc respectively.
- Can have allocation failures if too big of a buffer is requested to be allocated.
- You would use the heap if you don't know exactly how much data you will need at run time or if you need to allocate a lot of data.
- Responsible for memory leaks.

Example:

```c
int foo()
{
  char *pBuffer; //<--nothing allocated yet (excluding the pointer itself, which is allocated here on the stack).
  bool b = true; // Allocated on the stack.
  if(b)
  {
    //Create 500 bytes on the stack
    char buffer[500];

    //Create 500 bytes on the heap
    pBuffer = new char[500];

   }//<-- buffer is deallocated here, pBuffer is not
}//<--- oops there's a memory leak, I should have called delete[] pBuffer;
```

[Ref](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap?rq=1)