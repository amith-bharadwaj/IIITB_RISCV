# RISCV_ISA

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

Now let us observe the output using spike

```
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
```

![image](https://github.com/amith-bharadwaj/RISCV_ISA/assets/84613258/f4c3d309-97ba-4830-9b16-65b2612d4d61)


</details>

