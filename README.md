# Assembly x86-64 gcc 12.2

## Getting started

First of all, one must have knowledge of something -

- always push rbp in the first line of the code.
- pop that rbp just before return (when the code gets fully executed)
- mov rbp rsp in the 2nd line of any function of the code.
- we use registers as variables here
- registers are:
- **1. eax** - used by a number of arithmetic operations (accumulator)
- **2. ecx** - used as counter
- **3. esp** - stack pointer
- **4. ebp** - base pointer
- **5. edx** - acts as a accumulator's backup
- **6. ebx** - used to address memory or hold data
- if we are assigning a integer, we can use hexadecimal. hexadecimal are written with the leading '0x'.
- here all registers are 32 bits, which means they can hold decimal values from 0 to 2147483647. But what if we require to give only small value, in that case it is best to use the low-order 8 bit of the GPRs.
further table for GPRs refer [here](https://resources.infosecinstitute.com/topic/registers/)
- we need two things to create any variable: i. the size of the variable which will be allocated in memory ii. the value of the variable
- this is how we can assign size:
DWORD PTR - it is a 32bit declaration of a variable. (it isn't necessary to write)

## Instructions
### Data Movement Instructions
- **mov** - It basically copies the data item referred to by its first operand(register contents, memory or a constant value) into the location referred to by its second operand(register or memory)
SYNTAX: mov <first_operand> <second_operand>

- **push** - It places its operand onto the top of the hardware supported stack in memory.
SYNTAX:\
push <reg32>
push <mem>
push <con32>

EXAMPLES:\
push %eax - push eax on the stack
push var(,1) - push the 4 bytes at address var onto the stack

- **pop** - It just pops out from stack
SYNTAX:\
pop <reg32>

### Arithmetic and Logic Instructions
 - **add** - Integer addition
 SYNTAX:\
 add <first_operand> <second_operand>

 - **sub** - Integer substraction
 SYNTAX:\
 sub <first_operand> <second_operand>

 - **inc, dec** - Increment, Decrement
 SYNTAX:\
 inc <operand>
 dec <operand>

 Further read [here](https://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html#:~:text=Modern%20x86%2Dcompatible%20processors%20are,that%20specify%20addresses%20in%20memory.)

 ### Control Flow Instructions

- **jmp** - Transfers program control flow to the instruction at the memory location indicated by the operand.
 SYNTAX:\
 jmp <label>

 EXAMPLE:\
 jmp begin - Jump to the instruction labeled `begin`.

 - **cmp** - Compare the values of the two specified operands, setting the condition codes in the machine status word appropriately.

 SYNTAX:\
 cmp <first_operand> <second_operand>

 Further more [here](https://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html#:~:text=Modern%20x86%2Dcompatible%20processors%20are,that%20specify%20addresses%20in%20memory.)

 ## Simple Hello world program

 - In C

 ```bash
 #include <stdio.h>
int main()
{
    printf("Hello World!");
    return 0;
}
```

- in x86-64 gcc 12.2

```bash
.LC0:
        .string "Hello World!"
main:
        push    rbp
        mov     rbp, rsp
        mov     edi, OFFSET FLAT:.LC0
        mov     eax, 0
        call    printf
        mov     eax, 0
        pop     rbp
        ret
```

- `".LCO"` is the memory address where the string "Hello World!" beings.
- edi is the top of the stack, this code assumes that the value in edi is a memory location.
- The `"FLAT"` refers to the adress space: [here](http://en.wikipedia.org/wiki/Flat_memory_model)
- `"call prinft"` calls the printf function.
- `"ret"` returns the function.
