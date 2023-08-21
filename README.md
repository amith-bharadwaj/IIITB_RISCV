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

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/bad4f649-b01c-43b1-8e9c-6e8c0ce9ec43)


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/33442b80-9a8c-409d-a963-a95e0facf416)


![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/080272e0-d3ef-479c-b3e3-f2f865fdbdff)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/e8a0fa5a-2732-42c8-b7bb-852a2c98ca92)

![image](https://github.com/amith-bharadwaj/IIITB_RISCV/assets/84613258/7e7452c8-ca42-4e59-8beb-917e658813b0)


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
