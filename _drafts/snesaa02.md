---
layout:     post
title:      "SNES Assembly Adventure 02: Basic Assembly Programming"
date:       2018-07-26
excerpt:    "Continue your Assembly Adventure and learn your first 65816 opcodes"
tags:       [SNES Assembly Adventure, assembly, programming, SNES, tutorial]
feature:    /assets/snesaa/01/saa01_featurecard.gif
comments:   true
---
# FIRST DRAFT

Welcome back, Adventurer. I hope you are ready to continue your quest. Last time we set up a development environment to write and test our SNES games. In this article, we will have a closer look at the 65816 microprocessor and how it works. Then we will write some simple game logic and test it in an emulator.

Quick Refresher: Binary and Hexadecimal Numbering System
If you have some programming experience you're probably already familiar with binary and hexadecimal numbers. If not, watch this video as a quick refresher:

[[collin video]]

To distinguish binary and hexadecimal numbers I will from now on prefix binary numbers with the percent sign % and hexadecimal numbers with a hash #:

[[numbers]]

This is also the way to declare numbers in assembly source code. So you will see this a lot from now on.

### The 65816 Microprocessor
This is the heart of the SNES. Everything we do from now on will revolve around the 65816 16-bit microprocessor (at least, until we get to audio). The 65816 is the successor to the 6502 and 65C02 (an improved version of the 6502) 8-bit microprocessor. A highly successful and wide-spread microprocessor used in a range of computers like the Commodore 64 or the original NES. Actually, the 65816 instruction set is a superset of the 65C02. So (almost) any program written for the 65C02 will also run on the 65816. Keep this in mind since it is important when we talk about emulation and native mode in a moment. I won't go too deep into the history of the 65816 - there are tons of resources on the web about it.

To be precise, the CPU in the SNES is a Rico 5A22, a custom chip developed by Nintendo that adds certain features to the 65816. We will use these features in later articles.

So, what can the 65816 do for us? It will take zero, one, or two operands and perform an operation (on them). Here is a basic list of the operations it can perform:
Addition, Subtraction, Logical operations(AND, OR, XOR), Move data to/from memory, Compare numbers, Jump within the code
That's basically it. There are no high-level functions like printf() or sqrt(). It can't even multiply or divide! This is actually very important to understand when programming in assembly: A microprocessor does only very basic numbers crunching. Any high-level concepts like functions, strings, variables, etc. have to be implemented by the programmer.

Now, let's look at the basic architecture of the 65816:

[65816 architecture image]

The 65816 has three registers we can use: A, X, and Y. A is called the Accumulator, X and Y the Index Registers. A register is a very fast piece of memory inside the microprocessor. A register can hold 16 bits, or two bytes (this is not entirely true; actually, we have to switch them to 16-bit first, more on this in a moment). So we have three registers we can use. If you think, "Well, that's not a lot", you're perfectly right. The 6502 and 65816 were notorious for having only three working registers. In contrast, the Motorola 68000 microprocessor (sometimes called 68k) used in the Sega Genesis/Mega Drive has 16 registers each 32 bits wide! Speak about what Nintendon't. If you're interested in why that is, you can read up on RISC and CISC microprocessor on Wikipedia.

Usually, the programmer will load certain values into the registers, and the microprocessor will operate on these values. It is important to note that not every operation can be performed on every register. For example, we can only add a number to the number in register A (hence the name Accumulator). In fact, most arithmetic or logic operations are limited to the accumulator. But more on that later.

Next, there are six special purpose registers:
* Direct Page Register (D): used for direct page addressing, holds 16 bits
* Data Bank Registers (DBR) and Program Bank Register (PBR): used for addressing memory, hold 8 bits each
* Stack Pointer (S): points beyond the last item pushed to the stack, holds 16 bits
* Program Counter (PC): holds the address of the current line of code, holds 16 bits
* Processor Status Register (P): holds the state of the processor after the last instruction

We will discuss each register in detail when appropriate. For now, we will only look at the Processor Status Registers:
* N: Negative flag. Is set when the result of the last operation is a negative number
* V: Overflow flag. Is set when the last operation results in an overflow.
* M: Memory/Accumulator Select flag. Controls the size of register A, the accumulator. If set, the accumulator will be 8-bit, else 16-bit.
* X: Index Register Select flag. The same as the Memory/Accumulator Select flag, but for the X and Y registers.
* D: Decimal Mode flag. Will select whether the 65816 operates in decimal mode. This is disabled for the SNES, so you don't need to care about this flag.
* I: IRQ Disable flag. Controls whether the processor will react to an interrupt request. Interrupts will be covered in a later article. Think of them as external requests to the processor to execute a certain procedure/function.
* Z: Zero flag. Is set if the result of the last operation was zero.
* C: Carry flag. Is set if a carry occurred during the last operation.
* E: Emulation flag. This controls whether the processor is operating in emulation or native mode.

Generally, we say a flag is set when it is 1, and clear when it is 0. You might wonder why the Emulation flag is drawn above the Carry flag. This is because we cannot access the Emulation flag directly.

### Emulation and Native Mode
Earlier I told you the 65816 instruction set is a superset of the 65C02 instruction set. When we turn on/reset the 65816 it starts in emulation mode. In emulation mode, the 65816 acts like its predecessor the 65C02. This means we can only use the instruction set of the 65C02 and not the extended 65816 instructions. This feature was meant to guarantee backward compatibility. If we want to use the extended functionality of the 65816 (new instructions, addressing modes, 16-bit registers(!), 24-bit address bus, etc.) we first have to switch to native mode. One thing important to remember here: while in emulation mode, the registers of the 65816 are only 8 bits wide, not 16! Actually, once the 65816 has started, we first need to switch it from emulation to native mode. And then we need to tell it explicitly that we want to use 16-bit registers by manipulating the M and X flag of the processor status register.  So keep in mind: The 65816 starts in emulation mode. To use its full 16-bit powers, we need to switch to native mode first.

This might sound complicated, but actually, it only takes three simple instructions. We will not do this in this article yet, but in the next article, we will start wielding the full 16-bit powers of the 65816. For now, think that A, X, and Y can hold a byte each.

A crucial part of any microprocessor is the ALU, the Arithmetic Logic Unit. It handles all arithmetic and logical operations. The exact inner workings of it are not important to us. All you need to know is that it will execute the instructions and update the processor status register accordingly.

Those are the internal components of the 65816 microprocessor. To communicate with other parts of the system it uses two buses:

* The Address Bus: this bus is 24 bits wide, the 65816 can address up to 16 Megabytes
* The Data Bus: this bus is 8 bits wide, this bus actually moves data between the processor and memory.

Whenever the processor wishes to load or store data in memory, it will first put the address on the address bus and then read or write the data through the data bus. We will look at this process in more detail shortly.

Before we proceed, let's take a second and summarize what we know so far about the 65816 microprocessor:
* It has three working registers:
	* The Accumulator, or A register
	* The Index Registers, X and Y
* It has six special purpose registers:
	* Data and Program Bank Registers
	* Direct Page Register
	* Stack Pointer
	* Program Counter
	* Processor Status Register
* A 24-bit address bus to address up to 16 Megabytes
* An 8-bit data bus to write or read data to/from memory

This general overview should be enough for now. We will return to each register in more detail when we cover its function and purpose. Let's finally get down to business and write some code. This will clarify some of the 65816 architecture's details.

### A Simple Introduction to 65816 Assembly
When programming in assembly, there are two concepts that are key to writing fast and efficient assembly code: Understanding the microprocessor's addressing modes, and how each operation affects the processor status register. I have described the processor status register in general above. I will explain each flag in more detail as we proceed through this series of articles. To this end, I will introduce you to the various instruction codes and addressing modes of the 65816 one by one. Also, remember that the 65816 starts in emulation mode. So for now, registers can only hold 8 bits, not 16.

#### Your First Opcodes
Microprocessor instructions are often called opcodes. The shortcuts we use in assembly source code are called mnemonics. In most assembly languages mnemonics consist of a three letter abbreviation of the instruction/opcode it represents. Note that these terms are often used interchangeably.

The very first two opcodes you will learn the most basic: LDA will load a value from memory into the accumulator. STA will store the content of the accumulator in memory:

[lda : LoaD Accumulator]
[sta : STore Accumulator]

The 65816 has a total of 24 addressing modes. The first two are called immediate and absolute addressing mode.

Immediate addressing is used for data that is constant throughout the program. That means the value loaded into a register is not taken from memory but from a constant. We prefix the value we load into a register with a hash mark(#) to signal immediate addressing:

[gist immediate addressing]

In absolute addressing mode, we tell the opcode explicitly where to load from or store to the data in the register. Unlike immediate addressing this actually moves data from or to memory:

[gist absolut addressing]

You might be wondering why we use 16-bit addresses even though I told you the 65816 has a 24-bit address bus. This is because the 65816 calculates the final address by combining the address given by the opcode and the Data or Program Bank Register:

[address register]

The high byte of a 24-bit address is often called the address bank, while the middle and low bytes are called the address offset. For better readability, we separate the bank and offset address by a colon: $01:1A53 is the same as $011A53. The Data Bank Register and Program Bank Register are set to $00 on startup/reset. Those registers can be manipulated by special instructions only. For now, we will only use the memory space from $00:0000 to $00:FFFF, which equals 64 Kilobytes or one page or bank of memory.

[addressing modes image]

#### More Opcodes
Now, only loading and storing data won't get us very far. So let's introduce four more opcodes, one that actually manipulates register data:

[cmp : CoMPare accumulator]
[bcc : Branch if Carry Clear]
[clc : CLear Carry flag]
[adc : ADd with Carry]

The first one, CMP, will compare the value in the accumulator to another. CMP again can use immediate or absolute addressing mode:

[cmp gist]

Earlier I told you about the importance of understanding how opcodes affect the processor status register. CMP will set or clear the carry flag depending on the result: If the value in the accumulator is smaller than the value we compare it to, the carry flag will be clear. If the value in the accumulator is equal and greater than the compare value, the carry flag will be set. This behavior can be used to implement something similar to conditional expressions or if-else expressions.

Enter your first branch instruction, BCC. This opcode will check whether the carry flag is set or clear. If it is clear, the program will jump or branch to the label specified in the opcode. Labels are a useful tool to make our code more readable. Instead of using fixed addresses like $124A we let  the assembler replace our labels with the actual address:

[gist labels]

For now, think of labels as an alias for a given address.

Let's clarify this with a simple example. Say we want to check whether the value in the accumulator is greater than 64:

[bcc example gist]

 The next two opcodes are almost always used together. The first, CLC, clears the carry flag. So after the opcode is executed the carry flag will be cleared to 0. That's it.

Now, ADC, or ADd with Carry, will execute an addition on the accumulator. Again we can either use immediate or absolute addressing mode:

[adc gist]

Why do we need to clear the carry flag before an addition? The reason is that to get a correct result from a binary addition we need to clear the carry flag beforehand. If you're unfamiliar with binary arithmetics, read this.

You now know six opcodes and two addressing modes. These are enough to write some simple game logic, as we will do now. Keep in mind that there are more addressing modes to come and not every opcode can utilize every addressing mode.

#### Some Simple Game Logic
Let's finally write some useful code. Say we want to check whether the player has collected 100 coins and therefore gains an extra life. In C it might look like this:

[C gist]

Pretty straightforward. Now, let's do the same in assembly:

[assembly gist]

Wow, this looks way more complicated. Let's have a closer look.
Lines 4 through 7
First, we arbitrarily choose two memory locations to store the number of coins. For simplicity, we choose $00:0000 for the number of coins, and $00:0001 for the number of lives. Next, we store the starting values. The player starts with 0 coins and 3 lives.
Lines 12 through 14
This is the crucial part of this example. These three opcodes implement a behavior similar to a conditional statement or if-clause. First, we load the number of coins into the accumulator. And then compare it to 100. As explained earlier, the CMP opcode will modify the carry flag: If the value in the accumulator is smaller than 100, the carry flag will be clear, else it will be set. Next, BCC will check whether the carry flag is clear. If it is (so the number of coins is less than 100) the program will branch to the Done label and skip the code in lines 15 through 20. If the carry flag is set (so the number of coins is equal or greater than 100), then nothing happens and the program continues execution at line 15.
Lines 15 through 20
This part of the code will reset the number of coins to zero and increase the numbers of lives by one. This is pretty straightforward. We load the accumulator with the value $00 and store it in memory at $00:0000 where we keep track of the number of coins. Next, we load the current number of lives into the accumulator. Then we clear the carry flag in preparation for the addition. We add $01 to the value in the accumulator, and finally store the new number of lives back into memory at $00:0001.

I hope this simple example wasn't too hard to follow. If you have any questions, use the comment function below and I'll try and help.

Now, this code is really hard to read. There are a lot of numbers that can easily be confused. It is not directly clear what they do. Let us improve this code with labels to make it more readable.

#### Improving the Code with Labels
Labels are a convenient way to improve the readability of assembly code. We will replace the memory locations where we store the number of coins and lives with labels:

[improved assembly code gist]

This looks better than before. This also demonstrates another advantage of labels: Say we later in the development cycle determine that we need to move the memory location of the values of coins and lives. The only thing we need to change is the labels to accommodate the new memory locations without touching the rest of the code.

### Conclusion
This concludes this section and article about basic 65816 assembly programming. I hope this wasn't too dry. Some concepts like addressing modes can be quite confusing to the beginner. The next article will go into more details about the data and program bank register, how they affect addressing, and how to manipulate them.

In the next article, I'll show you how to build the above example with cc65 and run it in the bsnes+ emulator. Then we will expand this game logic to finally display a sprite.

As always, if you have questions or need any clarifications, please use the comment function below and I'll try and help.

### References and Links
