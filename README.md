# RISC-V-based-MYTH
This project was done as a part of RISC-V based MYTH (Microprocessor for You in Thirty Hours).

## Description
This repository is a summary of 5-day workshop on RISC-V based MYTH (Microprocessors for You in Thirty Hours).In this workshop, we learn about RISC-V ISA and geta hands-on experience on designing a RISC-V based core in verilog.

## Day 1: Introduction to RISC-V ISA and GNU compiler toolchain
Day-1 satrts with a brief introduction of RISC-V ISA and bsaic keywords used in teh architecture. We look into how the ISA helps the applications on the computer communicate with the hardware. In ISA flow, the app code (high level language) is converted to instructions based .exe file using a compiler which in turn is converted to binary using an assembler. The compiled instructions are based on the architecture type the hardware is based on and it is an implementation of RTL using hardware Descriptive Language (HDL).

![ISA_flow](https://github.com/rkuram/RISC-V-based-MYTH/blob/main/Images/Day1_1.PNG)

Different instructions in RISC-V ISA in this workshop are mainly categorized into:
- **Pseudo Instructions** - mv, ret
- **Base Integer Instructions (RV64I)** - lui,addi
- **Multiply Extension (RV64M)** - mulw, divw
- **Single and Double floating point Extension (RV64F and RV64D)** - flw, fmv

Any RISC-V cpu core has the Base Integer Instructions and if the CPU core that implements all the the above mentioned instructions, it is categorized as RV64IMFD. An application program uses Application Binary Interface(ABI) to access registers.

We then look into Integer number representation, where we learn how to convert a decimal number to binary. the 64 bit integer is called a doubleword which consists of 2-word/ 8 bytes where 4 bytes constitutes a word and 8 bits constitutes a byte. A RISC-V doubleword can represent '0' to '(2 <sup>64</sup> - 1)' unsigned numbers/positive numbers. To get a negative of a number in binary, we get Two's complement of the postive number. The Two's complement of a number is inverting the bits of the binary number and adding 1 to the lsb. An Integer is positove if the MSB is '0' and negative if the MSB is '1'. Based on this we get to know that, a RISC-V doubleword can represent '0' to '(2 <sup>63</sup> -1)' positive numbers and '-1' to '(-2 <sup>63</sup> )' negative numbers.

### Lab:
In ths lab wee look into how to compile a c program in a RISC-V based compiler.
we write a simple c-program to add n numbers named [**sum1ton.c*"](https://github.com/rkuram/RISC-V-based-MYTH/blob/main/Source/sum1ton.c)
We then compile the above program using a RISC-V GNU compiler by typing the following commands.
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
- The `-O`  argument represents the different level of optimizations for the compiler to use. (ex: -O1, -Ofast)
- The arguments `-march` selects the architecture to target. This controls which instructions and registers are available for the compiler to use, which is rv64i (64-bit integer instructoins)
- The argument `-mabi` selects the ABI to target. This controls the calling convention (which arguments are passed in which registers) and the layout of data in memory, which is lp64 (long and pointers are 64-bits long)

To look at the assembly code, we type in the following commands.
```
riscv64-unlnown-elf-objdump -d sum1ton.o
```
The argument `-d` is used called dissamble and `objdump` is called object dump.

Spike Simulation, to run the code
```
spike pk sum1ton.o
```
Spike Debug
```
spike -d pk sum1ton.o
```
To run the process to a certain byte address in the debugger, we use
```
: until pc 0 108b0
```
The number 108b0 is the byte address, which we use in one of the MCQ.

- **Results from sum1ton.c**

![Day1_1](https://github.com/rkuram/RISC-V-based-MYTH/blob/main/Images/instr_printf_O1.PNG)

![Day1_2](https://github.com/rkuram/RISC-V-based-MYTH/blob/main/Images/instr_main_Ofast.PNG)

![Day1_3](https://github.com/rkuram/RISC-V-based-MYTH/blob/main/Images/lab_answers.PNG)

Later in this lab, we write a c-program to calculate the highest and lowest numbers represented by long long int.
The data types and the format specifier are given in the table below
| Data Tyes | Memory (bytes) | Format Specifier |
| :---      |      :---:     |       :---:      |
| `unsigned int` | 4 | %u |
| `int` | 4 | %d |
| `unsigned long long int` | 8 | %llu |
| `long long int` | 8 | %lld |

**signedHighest.c**
```
#include <stdio.h>
#include <math.h>

int main(){
  long long int max = (long long int) (pow(2,63)-1);
  long long int max = (long long int) (pow(2,63)*-1);
  printf("highest number represented by long long int is %lld\n", max);
  printf("lowest number represented by long long int is %lld\n", max);
  return 0;
}
```
- **Results from signedHighest.c**

![Day1_4](https://github.com/rkuram/RISC-V-based-MYTH/blob/main/Images/lab_answer_2.PNG)
