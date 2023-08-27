# IIITB_RISCV


<details>
    
<summary>DAY-0</summary>

# DAY-0

# Installation of Tools

Follow the below commands for installation of tools.

```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh

cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install

gedit .bashrc
export PATH="/home/amith/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" 
source .bashrc
```

</details>

<details>
    
<summary>DAY-1</summary>

# DAY-1
## Introduction to RISC-V ISA and GNU compiler toolchain

RISC-V (pronounced "risk-five") is an open-source instruction set architecture (ISA) that is designed to be simple, extensible, and modular. It is often referred to as a "free and open RISC instruction set architecture," as it is not encumbered by patents or proprietary restrictions, allowing anyone to use, modify, and contribute to its development.

The C program is compiled into RISC-V assembly language program, this assembly language program is converted into machine level program, which is binary language program. These binary bits will be executed into this particular layout seen in the image below.The Risc-V architecture is implemented by the given RTL (picorv32 cpu core).

![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/ff4b5316-9ea3-4eb9-bc8d-1dc3c3222c02)

The application software will run on the hardware by the given flow.Apps enter into the block of system software, this block converts into the binary language, the system software block contains OS,Compiler and Assembler.OS handles IO operations,allocates memory and performs low level system functions.
The output of the OS are small chunks of C,C++ or Java language, these are taken by the compiler and converted into instructions.Depending on the hardware the format or syntax of the instructions will change. Then the assembler will convert these instructions into binary language program.This binary language is fed to the hardware.Then RTL implementation is done and the synthesized netlist is obtained.

![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/0df7f17d-6217-4ebe-9496-4283764b8803)

![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/982be285-c03e-4496-aa2d-40e9c3fc454e)

## LAB work for RISC-V software toolchain

Let us execute a simple program which computes sum from 1 to a given number N.

```
gedit sum1ton.c
gcc sum1ton.c
./a.out
```

![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/b1799e88-2b3b-40c6-a8cd-1bc384aaffdc)


![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/2405e1cf-0f84-4390-a06d-b97ebbc33df2)

Now let us compile it with risv compiler

```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/531aec9c-25d6-4fc7-89eb-b2fc4b312e47)

It will generate the file sum1ton.o, let us go to another tab and run the following commands.

```
riscv64-unknown-elf-objdump -d sum1ton.o | less
```
It will give us a bunch of assembly language code.

We need to look for main section.

```
/main
```
Here we can see 15 instructions which came out when we used the previous commands.Since it is a byte instruction, it always increments by 4.

![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/17fd6343-fbe1-43e1-816c-e7e5d0766c6c)

Now, let us run the command with -ofast.

```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c

```
Here we can see that 12 instructions were produced.

![image](https://github.com/amith-bharadwaj/iiitb_asic_class/assets/84613258/980923ad-7fee-4096-b298-8c1797c33bdd)

### Spike simulation and Debug.

Now let us observe the output using spike

```
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
```

![image](https://github.com/amith-bharadwaj/RISCV_ISA/assets/84613258/f4c3d309-97ba-4830-9b16-65b2612d4d61)

The below command is used for debuging line by line.
```
spike -d pk sum1ton.o
```

**LUI**: The "LUI" instruction in RISC-V stands for "Load Upper Immediate." It's used to load an immediate value into the upper 20 bits of a 32-bit register, with the lower 12 bits being filled with zeros. Here's the general format of the LUI instruction:

![image](https://github.com/amith-bharadwaj/RISCV_ISA/assets/84613258/0f14b940-23cd-477c-b36e-99b87a8edff8)

**ADDI**: The "ADDI" instruction in RISC-V stands for "Add Immediate." It's used to add an immediate value to the value in a register and store the result back in the destination register. Here's the general format of the ADDI instruction:

![image](https://github.com/amith-bharadwaj/RISCV_ISA/assets/84613258/887aa66e-dfad-4fef-b8b0-642c3e80f58a)

## 64 Bit Number System for unsigned numbers and signed numbers

In a 64-bit computer architecture,

**Byte:**
        A "byte" in a 64-bit architecture consists of 8 bits, just like in other architectures.
        Each byte can represent a range of values from 0 to 255 (2^8 - 1).
        Bytes are fundamental units of storage and data representation, commonly used for characters, numbers, and other small data units.
        For example, a single ASCII character like 'A' is represented by a byte.

**Word:**
        In a 64-bit architecture, a "word" typically refers to a unit of data that is 64 bits, or 8 bytes.
        This size is often chosen to match the size of the processor's general-purpose registers, enabling efficient processing of data.
        Words are commonly used for integer arithmetic, memory addressing, and data manipulation operations.

**Double Word:** In a 64-bit architecture, a "double word" is a unit of data that is twice the size of a word, hence 128 bits or 16 bytes.Double words are used for larger data structures, floating-point numbers, and certain specialized operations that require more storage space.

        
![image](https://github.com/amith-bharadwaj/RISCV_ISA/assets/84613258/b2e2a8db-c052-4aac-ae13-22d5780536ee)

Here below we can see the representation of unsigned numbers in 64 bit architecture.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/38c73c57-49dd-4498-b9c0-27e5186db566)

The format and memory for different data types are given below.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/094579d4-2127-4602-a157-21563d46535f)

</details>

<details>
    
<summary>DAY-2</summary>

# DAY-2
## Application Binary Interface

The Application Binary Interface (ABI) for the RISC-V architecture defines a set of rules and conventions for the interaction between software components at the binary level. It encompasses how functions are called, how data is represented and manipulated, how memory is managed, and how system calls are made. The RISC-V ABI ensures compatibility and interoperability between different software modules, making it possible for programs to work seamlessly on different systems that adhere to the same ABI.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/1f14cdec-d0cc-498f-b0a6-4f8a1e3f118a)

### Memory Allocation 

There are two different ways to load the data into registers,the data can be loaded directly to registers but as we dont have large number of registers, we can load the data into the memory and then from memory we can load the data into the registers.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/982bd744-0e8d-45ba-b862-419dcdd9e2a1)

RiscV belongs to little-endian memory addressing system.Little-endian memory addressing is a way of organizing and storing data in computer memory where the least significant byte (LSB) of a multi-byte value is stored at the lowest memory address, while the most significant byte (MSB) is stored at a higher memory address.This byte order is opposite to big-endian memory addressing.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/53829752-ee0f-4170-a491-724a64ded03f)

**ld:** This mnemonic stands for "load double-word." It's an instruction used to load a 64-bit (8-byte) value from memory into a register.

**x8:** This is the destination register where the loaded value will be stored. In RISC-V assembly language, registers are denoted by the "x" prefix followed by a number (e.g., x0, x1, x2, ..., x31).

**16:** This is the immediate offset value, which indicates the offset from the address stored in register x23. The offset is added to the address in x23 to calculate the memory address from which the value will be loaded.

**(x23):** This indicates that the address to be used for loading the value is stored in register x23. x23 is the base register, and the offset is added to its value to compute the effective memory address.
    
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/82e52cc8-7eb7-49dc-b5be-7993857d3412)

The format of instruction can be seen below.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/5bb365b1-f46c-4e87-a65c-fc398499e059)

The add instruction below performs the addition operation by adding the values stored in registers x24 and x8 together. The result of the addition is then stored back in register x8, overwriting the previous value.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b9cbd593-8d84-4d53-9701-7b8b10baa843)


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/d17f8452-72fa-496c-bea4-fa210881d1fc)

The below image shows the different ABI names,registers and its usages.
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/d2614bb0-4fb3-4fbb-b747-437924e937fb)

## Lab Work using ABI function calls

Let us perform the lab to rewrite c program in asm language.The main c program passes a0 and a1 to ASM block and the ASM returns a0 back to the main c program.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/c231b772-fe51-484c-b5c6-3fd9c8ee9478)

The algorithm for the operation of program can be seen below.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/1b55a7fc-b712-4c4a-b659-1a659298a999)

This is 1to9_custom.c file 

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/4da8461e-3ac1-4073-abda-b0268e556984)

This is load.S file

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/68ef46f8-b98c-42cb-b4c6-d5900efb0447)

The execution of the program is performed.
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.S
spike pk 1to9_custom.o
riscv64-unknown-elf-objdump -d 1to9_custom.o |less
```

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/9d5624c4-ab71-4b22-9a83-e2a330cc4191)

## Lab to run C program on RISC-V CPU
We will load the hex format of C Program to the RISC-V CPU which is written in verilog and the end result is displayed.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/173e1d5e-2d97-4370-b67b-4acd8092f082)

Follow the below commands to run the program.

```
chmod rv32im.sh
./rv32im.sh
```
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/11ddb3bb-27fd-4ae0-b01b-a5b25084ebab)


</details>

<details>
    
<summary>DAY-3</summary>

# DAY-3
# Digital Logic with TL-verilog and MakerChip

**Logic Gates** : Logic gates are fundamental building blocks of digital circuits that perform logical operations on one or more binary inputs (usually represented as 0 or 1) to produce a binary output. These gates implement basic logical functions and are the foundation of all digital systems and computers.

![Screenshot from 2023-08-20 22-45-39](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/2d0c3c6b-cce4-4b0c-aa62-257f9a5c623c)

**Combinational Circuit** : A combinational circuit is a type of digital circuit in which the output is solely determined by the current input values, without any consideration of previous inputs or circuit states. In other words, the output of a combinational circuit depends only on the instantaneous values of its input signals, and there is no concept of memory or feedback within the circuit. Combinational circuits are widely used in digital systems for tasks such as arithmetic operations, data manipulation, and logic processing.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/c1554765-ea65-4b0d-a4c0-3b4b6d9a6992)

Adder

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/e923ea4f-f6ba-4ea5-9989-301f031339ca)

Boolean Operator

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/97c73212-01ba-44e3-a320-91baac99dad9)

Multiplexer

![Screenshot from 2023-08-20 22-58-39](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/98e0e463-386e-43ff-ab0d-fc75932a46d2)

Chaining Ternary Operator

![Screenshot from 2023-08-20 23-04-09](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/911f4d64-9c85-465b-adcf-c3633c69015f)

#### MakerChip

Makerchip is a popular online Integrated Development Environment (IDE) primarily focused on digital design and hardware description languages (HDLs). It's designed to facilitate the design, simulation, and verification of digital circuits and systems.

**Pythagorean Example**

![Screenshot from 2023-08-20 23-29-46](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b2f7e54d-ad08-49c5-a14a-1cc687a3ae64)

#### Combinational Logic

Task 1: Inverter

![Screenshot from 2023-08-20 23-39-11](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/4109d792-b6d8-41b9-a04c-74a23ba03e24)

Task 2: AND Logic circuit 

![Screenshot from 2023-08-20 23-41-10](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/17943c26-a4dc-49e1-ae35-8d2dfaca1bde)

Task 3: Vectors

$out[4:0] will create a vector of 5 bits.Arithmetic operators on vectors will act as binary numbers. 

![Screenshot from 2023-08-20 23-45-49](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/167b60b0-0fb1-4308-ae16-8933b16e2856)

![Screenshot from 2023-08-20 23-47-58](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/bdf843a6-d4cb-4016-9896-419633010589)

Task 4 : Multiplexer

![Screenshot from 2023-08-20 23-48-53](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/17e1db43-818d-4671-b71c-2d54c2e4e219)

![Screenshot from 2023-08-20 23-52-27](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b5eda7f8-1aa8-4f54-b040-ba49a6aae014)

Multiplexer using vectors

![Screenshot from 2023-08-20 23-54-36](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/598e0681-8812-454f-8866-a41a359f3170)

Task 5: Combinational Calculator

This circuit implements a calculator that can perform +,-,*,/ on two input values.

![Screenshot from 2023-08-20 23-59-43](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/790dca5b-14bf-4e9c-ac1a-cd45ada9c6f7)

![Screenshot from 2023-08-21 00-16-27](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/68075105-8e82-4f62-b808-37a66b3e38c7)

#### Sequential Logic

Sequential logic refers to a type of digital logic circuit or system in which the output is determined not only by the current inputs but also by the history of inputs.In sequential logic, the circuit contains memory elements such as flip-flops or registers that store information and maintain a state. This stored information influences how the circuit responds to subsequent inputs. The concept of time or clock signal is crucial in sequential logic, as it dictates when the stored information is updated and when new computations can take place.

Example: Fibbonaci series with Reset

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/c0612a9d-880e-4cd4-8f8d-0f04c23f98b7)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/d10be0fc-2f15-4e61-8ea0-12a6ac764e5c)

Task 1: Counter

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/55945da3-25dd-4766-a454-06ff83859664)

The TLV code for performing the counter operation is given below.
```
$cnt[31:0] = $reset ? 0 : (1 + >>1$cnt);
```
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/9ccc748c-75e7-4bed-aab6-f910616ac72e)

Task 2: Our task is to perform a sequential calculator operation.This calculator should remember the last result and use it for the next calculation.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/97287d00-b851-4044-925e-eddbba7bb0e8)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/173d9ac8-065e-4aab-b41b-318403e0c397)

## Pipelining

Pipelining in refers to a design methodology employed in digital integrated circuit (IC) development to enhance the throughput and performance of sequential operations. It involves breaking down a complex computation or task into smaller, discrete stages, each of which can be executed concurrently by separate hardware modules. These stages are interconnected in a serial manner, creating a pipeline that allows new operations to enter the pipeline before previous ones have completed.

The primary objective of pipelining is to minimize the idle time of hardware resources and maximize overall system throughput. By enabling multiple tasks to be processed simultaneously at different pipeline stages, the effective utilization of resources is improved, resulting in faster execution times for a sequence of operations.However,pipelining introduces additional challenges such as data hazards, control hazards, and inter-stage synchronization.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/bad4f649-b01c-43b1-8e9c-6e8c0ce9ec43)


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/33442b80-9a8c-409d-a963-a95e0facf416)


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/080272e0-d3ef-479c-b3e3-f2f865fdbdff)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/e8a0fa5a-2732-42c8-b7bb-852a2c98ca92)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/7e7452c8-ca42-4e59-8beb-917e658813b0)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/f3941be2-d161-4a4e-8637-61ec5c3e3e42)

Let us perform the pipelining of error conditions.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/dbfa03ed-45bb-46c5-a38a-7f595127295c)

Task: Pipelining of Calculator and Counter

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b2052e95-2767-4bff-928d-d1011e05c58a)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/37a6cdbc-1c84-47ba-b650-b99ef615c797)

Task: Cycle Calculator

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/f2ee4269-0a4d-4ea6-928d-8ee97759d746)
The TLV code is given below.

```
|calc
      @1
         
        $reset = *reset;
        $val1[31:0] = >>2$out[31:0] ;
        $val2[31:0] = $rand2[3:0];
        $sum[31:0] = $val1[31:0] + $val2[31:0];
        $diff[31:0] = $val1[31:0] - $val2[31:0];
        $prod[31:0] = $val1[31:0] * $val2[31:0];
        $quot[31:0] = $val1[31:0] / $val2[31:0];
        $valid = $reset ? 0 : (>>1$valid + 1);
      @2
        $out[31:0] = ($reset | ~($valid))  ? 32'h0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));

        
 
```

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/ff8e85d3-1f03-4433-ad5d-8e7b007ad678)

## Validity

In Transaction-Level Verilog (TL-Verilog), which is an extension of the Verilog hardware description language (HDL), "validity" refers to the concept of indicating whether a piece of data is valid or not. TL-Verilog is designed to facilitate high-level modeling and rapid design entry, particularly for transaction-level modeling.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/23133cdb-8b66-43a5-bd38-9d060f0f1df5)

### Clock Gating

Clock gating is a power-saving technique used in digital circuit design to reduce dynamic power consumption by selectively controlling the clock signal to specific circuit elements or modules. The primary goal of clock gating is to save power by stopping the clock signal from reaching parts of the circuit that are not currently active or performing useful computation.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/ad5ebcbb-6fff-406c-a6ed-38f640feb825)

Exercise 1: Distance Accumulator

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/2661aebd-ab3d-4791-a547-2c9b80f1a54f)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/8caeb14a-26b3-405e-bdff-0343e01ed905)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/4d147b37-36b8-44bd-9351-87e9b597973f)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/aa5e06e1-5c4e-4346-afd0-e9763a7130f6)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/46aff40e-5d8e-4de6-81c6-1fedd32c25c4)

Exercise 2: Cycle Calculator with Validity

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/58913fc5-c379-4fa5-8673-0ab4b24d792b)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/3cc36546-b378-49b4-92d6-1fcc40ef3b90)

Calculator with single value memory 

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/72abac6c-b4e7-45c5-840b-d688ab4a93e2)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/e718ffa0-e7da-488f-8ed4-cf532aaab61b)


</details>


<details>
    
<summary>DAY-4</summary>

# DAY-4


## RISC-V Micro Architecture

### RISC-V Block Diagram

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/9348ff29-26f3-4a53-8709-17a80284fab7)

1.The **Program Counter** (PC) serves as a specialized register within a CPU, tasked with keeping a record of the memory address pertaining to the upcoming instruction set for fetching and execution. It undergoes incremental updates as instructions are fetched, and it plays a role in indicating the address to the instruction memory, thereby facilitating the retrieval of the subsequent instruction within the program sequence.

2.The **Instruction Decoder** takes form as an internal circuit embedded within the CPU, responsible for the interpretation of machine instructions obtained from memory. It translates the binary representation of the instruction into control signals that oversee the functioning of other CPU components, ensuring the execution of the instruction itself.

3.Functioning as a storage module, the **Instruction Memory** safeguards the program's machine instructions. This section, often set as read-only, harbors the binary instructions that the CPU retrieves and deciphers. The program counter works in conjunction with the instruction memory to determine the address from which the following instruction should be retrieved.

4.The **Data Memory** operates as a storage component designated for containing data manipulated by instructions during program execution. Unlike instruction memory, data memory permits both reading and writing. It holds variables, data arrays, and other pertinent information crucial for the program's execution.

5.The **Arithmetic Logic Unit (ALU)** stands as a pivotal digital circuit situated within the CPU. It takes charge of conducting arithmetic and logical operations on data. Its repertoire spans various tasks such as addition, subtraction, multiplication, division, bitwise operations (AND, OR, XOR), and comparisons. The outcomes generated by the ALU fuel an array of computations as prescribed by the instructions.

6.The **Read Register File** functions as a constituent that stores an ensemble of registers employed to house data during instruction execution. Instructions often involve extracting data from these registers. Specific registers are designated by the instruction for data retrieval, and the data sourced from these registers can function as operands for operations executed by the ALU or other components.

7.The **Write Register File** shoulders the responsibility of capturing the outcomes of operations and storing them back into registers. Following the execution of an instruction, the outcomes are frequently inscribed back into the register file. This practice guarantees that the revised data stands ready for ensuing instructions.

The harmonious collaboration of these elements facilitates the execution of machine instructions within a CPU. The program counter guides the procedure of instruction acquisition, the instruction decoder translates instructions, the ALU carries out mathematical operations, the register files preserve data, and the memory components provide storage and retrieval of data. This orchestration permits the CPU to adeptly undertake the responsibilities stipulated by a program's instructions.

### Fetch and Decode

This is how the PC gets incremented and how the reset makes the PC to 0.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/c26c0489-262c-4bc2-8602-5557d9ff0ddc)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/8e1dcdd5-438d-48e5-b465-0abb054e2d57)

Let us execute the fetch program.
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/4dac3b48-3971-4cf1-8ce2-8a003334db9e)
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/8aea44a3-a2be-4fbf-9f45-7fc7e325022e)
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/bfe050f9-4fff-47b1-9084-c9d055e88a43)

Let us move on to the Decode logic.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/533b8b15-e779-4aba-8090-63b8a90337c8)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/6bad2712-fa7f-4708-be55-e183630c4532)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/6ea43706-85b7-4cac-a673-92f51e0696d7)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b5c38269-7322-44ae-8274-80a1b50b7db9)

### Risc-V Control logic

Let us perform the lab for register file read logic.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b08a7c53-e262-4eb9-a5dc-005d205851cd)
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/7d3303ab-2b25-4b9a-8449-facd54381566)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/b4ca4ab4-e62c-4f0b-b8c4-b0f75149c7a7)

Let us perform the lab for ALU logic
.
![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/1b0633b4-e2f4-43f6-835b-730309bda933)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/607fb581-fd50-4a9f-99a9-a166ebd730af)

Let us perform the lab for Register File Write Logic

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/df3f7bc7-b4d1-43d4-8f84-f1a9c6170067)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/9a0ffa28-126f-4184-8418-3a688ca135be)

#### Concept of Arrays and register files

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/312ceda0-d222-4118-bb8a-ffb848ed3573)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/7e72e1ca-4c78-44a3-b155-e84694540bd6)

#### Branch Instructions


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/2ed93123-4af1-4c6f-b089-44168d4d076b)

Branch instructions in the RISC-V architecture are used to alter the program flow based on certain conditions. They allow the program to jump to a different instruction address, often referred to as the target address, if a specified condition is met. Here's an explanation of the branch instructions you mentioned:

1.**BEQ** (Branch if Equal): The BEQ instruction compares two registers and branches to the target address if they are equal. It checks if the contents of two specified registers are the same, and if they are, the program counter is updated to the target address.

2.**BNE** (Branch if Not Equal): The BNE instruction also compares two registers but branches to the target address if they are not equal. If the contents of the two registers are different, the program counter is updated to the target address.

3.**BLT** (Branch if Less Than): The BLT instruction compares two signed integer values in registers and branches to the target address if the first value is less than the second value. It checks the signed comparison between two registers and jumps if the first register's value is less than the second's.

4.**BGE** (Branch if Greater Than or Equal): The BGE instruction compares two signed integer values in registers and branches to the target address if the first value is greater than or equal to the second value. It checks the signed comparison and jumps if the first register's value is greater than or equal to the second's.

5.**BLTU** (Branch if Less Than, Unsigned): The BLTU instruction compares two unsigned integer values in registers and branches to the target address if the first value is less than the second value. It performs an unsigned comparison and jumps if the first register's value is less than the second's.

6.**BGEU** (Branch if Greater Than or Equal, Unsigned): The BGEU instruction compares two unsigned integer values in registers and branches to the target address if the first value is greater than or equal to the second value. It performs an unsigned comparison and jumps if the first register's value is greater than or equal to the second's.

In all of these branch instructions, if the specified condition is met, the program counter is updated to the target address, resulting in the program branching to a different part of the code. If the condition is not met, the program continues with the next sequential instruction after the branch instruction.

These branch instructions are fundamental for implementing conditional execution, loops, and decision-making in programs written in the RISC-V assembly language. They provide the ability to create more flexible and dynamic program flows.

Let us perform the LAB for performing branch instructions.The verilog code is given below.
```
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;



      // YOUR CODE HERE
      // ...
      @0
         $pc[31:0] = >>1$reset ? 32'd0 : (>>1$taken_branch ? >>1$br_tgt_pc :  (>>1$pc+32'd4));
      @1
         //Instruction Fetch
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1
         //Instruction Decode
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001 ||
                       $instr[6:2] ==? 5'b11100;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         $opcode[6:0] = $instr[6:0];
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid 
            $rd[4:0] = $instr[11:7]; //rd - Destination Register
            
         $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
      @1
         //Register File Read
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
      @1
         //ALU
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
      @1
         //Register File Write
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
      @1
         //Branch Instructions
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         $br_tgt_pc[31:0] = $pc + $imm;
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
      //m4+myth_fpga(@0)  // Uncomment to run on fpga

   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule
```

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/0f400e61-3bf4-47c1-928f-a275162a7b40)

#### Test Bench

**Test bench** consists of a set of stimuli (input patterns or signals) that are applied to the design under test (DUT), along with a mechanism to capture and analyze the resulting outputs or responses. The purpose of a test bench is to simulate real-world scenarios and test various aspects of the design's behavior to ensure that it meets its intended specifications and requirements.


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/bd6d3cf6-d02e-4f5a-b05b-5be762d2af2b)

    
</details>

<details>
    
<summary>DAY-5</summary>

# DAY-5

### Pipelining 

These are the pipeline stages in a RISC-V CPU:

1.**Instruction Fetch (IF):** In this stage, the processor fetches the instruction from memory using the program counter (PC). The PC is updated to point to the next instruction.

2.**Instruction Decode (ID):** The fetched instruction is decoded in this stage. The decoder identifies the instruction type, determines the required operands, and prepares the control signals needed for subsequent stages.

3.**Execute (EX):** The ALU (Arithmetic Logic Unit) performs the actual computation or operation specified by the instruction. This stage can include arithmetic, logical, and comparison operations.

4.**Memory Access (MEM):** This stage handles memory-related operations. If the instruction involves memory access, such as loads or stores, it is executed here. Data is read from or written to memory.

5.**Write Back (WB):** In this stage, the result of the executed instruction is written back to a register. This stage ensures that the updated data is available for subsequent instructions.

By breaking down the execution process into these stages, the pipeline allows multiple instructions to be in different stages simultaneously. While one instruction is in the execution stage, another can be in the decode stage, and yet another in the fetch stage. This overlap increases the CPU's efficiency and throughput.


Here are some common hazards that can occur in pipelining:

**Structural Hazards:** These occur when multiple instructions in the pipeline compete for the same hardware resource at the same time. For example, if two instructions require the same functional unit (e.g., the ALU or memory unit) in the same pipeline stage, a structural hazard can occur. Proper resource allocation and scheduling are needed to avoid structural hazards.

**Data Hazards:** Read-After-Write (RAW) Hazard: This occurs when an instruction depends on the result of a previous instruction that has not yet been written back to the register file. Pipelining can cause the dependent instruction to start executing before the source instruction has completed, leading to incorrect results. Techniques such as forwarding (also known as data forwarding or bypassing) and stalling (inserting NOPs) are used to resolve RAW hazards.
        Write-After-Read (WAR) Hazard: This happens when an instruction writes to a register that a following instruction reads from. If the write occurs after the read instruction has entered the pipeline, it could lead to incorrect data being read. Proper scheduling and register renaming techniques can mitigate WAR hazards.
        Write-After-Write (WAW) Hazard: This occurs when two instructions write to the same register. If the second write completes before the first, it might lead to incorrect results. Register renaming can eliminate WAW hazards.

**Control Hazards:** These arise from the dynamic nature of program flow, such as branches and jumps. If a branch instruction is in the pipeline and its outcome is not yet known, subsequent instructions that have already entered the pipeline may need to be flushed if the branch outcome is different from the predicted outcome. Techniques like branch prediction (static or dynamic) are used to minimize the impact of control hazards.

**Memory Hazards:**
        Load-Use Hazard: This occurs when a load instruction reads data from memory and a following instruction uses the loaded data before it's available. Forwarding and stalling techniques are employed to handle load-use hazards.
        Store-Load Hazard: This happens when a store instruction writes to memory and a subsequent load instruction reads from the same memory location. Careful memory ordering and forwarding are required to address store-load hazards.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/a9c67956-7e48-4a10-9bf5-cc93d112953f)


### WaterFlow Logic Diagram

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/4859f845-58a4-4034-826f-446cf370f90f)

The working of waterflow diagram is shown below.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/7d8b6c21-75d1-47a3-9a44-ad65f8b8e855)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/a23fe745-4a47-4b2e-b428-33c9d4d8f268)

Let us do a lab to see which cycles are valid and which of them are invalid.

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/ccf3d0d3-cda4-469c-aeb5-5bc092411e51)
pipelined logic in the TL-Verilog code is given below:

```
\m4_TLV_version 1d: tl-x.org
\SV
   //  This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store the final result value to byte address 16
   m4_asm(LW, r17, r0, 10000)           // Load the final result value from adress 16 to x17
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
   
   |cpu
      @0
         $reset = *reset;
         
         //NEXT PC LOGIC       
         
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>3$valid_taken_branch ? >>3$br_target_pc :
                     >>3$valid_load ? >>3$inc_pc :
                     >>3$valid_jump && >>3$is_jal ? >>3$br_target_pc :
                     >>3$valid_jump && >>3$is_jalr ? >>3$jalr_target_pc :
                     >>1$inc_pc ;
      @1   
         $inc_pc[31:0] = $pc + 32'd4;
         
         
      @3
         //CYCLE VALID INSTRUCTIONS
         $valid = !(>>1$valid_taken_branch || >>2$valid_taken_branch || 
                    >>1$valid_load || >>2$valid_load ||  
                    >>1$valid_jump || >>2$valid_jump) ;
         
         $valid_load = $valid && $is_load ;
         
         $valid_jump = $is_jump && $valid ;
         
         
         
         //INSTRUCTION FETCH LOGIC
      @1 
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
         
         //INSTRUCTION TYPES DECODE   
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         
         //INSTRUCTION IMMEDIATE DECODE
         
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         `BOGUS_USE($imm)
         
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
            
         `BOGUS_USE($rd)
         
      @2
$is_jump = $is_jal || $is_jalr ;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         
       
         
      @3
  //INSTRUCTION DECODE
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         $is_load = $opcode == 7'b0000011;
         
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         
         $jalr_target_pc[31:0] = $src1_value +$imm ;
        
      @2
         //REGISTER FILE READ
         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
      @3
         //REGISTER FILE WRITE
         $rf_wr_en = ($rd_valid && $rd != 5'b0 && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = >>2$valid_load ? >>2$rd : $rd;
         $rf_wr_data[31:0] = >>2$valid_load ? >>2$ld_data : $result;
         
      @2   
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :  
                             $rf_rd_data1;
         
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                             $rf_rd_data2;
         
      @4
         //MINI 1-R/W MEMORY
         $dmem_wr_en = $is_s_instr && $valid ;
         $dmem_addr[3:0] = $result[5:2] ;
         $dmem_wr_data[31:0] = $src2_value ;
         $dmem_rd_en = $is_load ;
         
      @5
         //LOAD DATA
         $ld_data[31:0] = $dmem_rd_data ;
         
         
      @3   
         //ARITHMETIC AND LOGIC UNIT (ALU)
         
         $sltu_rslt[31:0] = $src1_value < $src2_value ;
         $sltiu_rslt[31:0]  = $src1_value < $imm ;
         
         $result[31:0] = $is_andi ? $src1_value & $imm :
                         $is_ori ? $src1_value | $imm :
                         $is_xori ? $src1_value ^ $imm :
                         ($is_addi || $is_load || $is_s_instr) ? $src1_value + $imm :
                         $is_slli ? $src1_value << $imm[5:0] :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_and ? $src1_value & $src2_value :
                         $is_or ? $src1_value | $src2_value :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_add ? $src1_value + $src2_value :
                         $is_sub ? $src1_value - $src2_value :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_srl ? $src1_value >> $src2_value[4:0] :
                         $is_sltu ? $src1_value | $src2_value :
                         $is_sltiu ? $src1_value < $imm :
                         $is_lui ? {$imm[31:12], 12'b0} :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 :
                         $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                         $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                         $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                         $is_sra ? {{32{$src1_value[31]}}, $src1_value} > $src2_value[4:0] :
                         32'bx ;
         
       
         //BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         
         $valid_taken_branch = $valid && $taken_branch;
                  
      @2
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;
         
         
         //TESTBENCH
         *passed = |cpu/xreg[17]>>5$value == (1+2+3+4+5+6+7+8+9) ;
         
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.
   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule

```

![Screenshot from 2023-08-25 14-46-09](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/03672a42-f54d-43af-be27-6b03cb1e4e8c)




</details>


<details>


    
<summary>REFERENCES</summary>

# REFERENCES

1. https://www.vsdiat.com/
2. https://github.com/kunalg123/vsdflow
3. https://teamvlsi.com
4. https://vlsiverify.com/verilog
5. https://github.com/stevehoover/RISC-V_MYTH_Workshop



</details>
