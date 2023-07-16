# Rosa File Format - Intermediate Representation (IR) Documentation

## Introduction
Intermediate Representation (IR) is a crucial aspect of the Rosa.
It serves as a platform-independent and architecture-agnostic representation of
executable code within the Code Section. This document provides an overview of
the purpose, benefits, and usage of the IR in the Rosa file format.

## Purpose of IR
IR within the Rosa acts as a bridge between
the high-level source code of the Violet application and the low-level machine
code that runs on specific hardware platforms or architectures. It captures the
essential semantics, structure, and behaviour of the program at a higher level
of abstraction.

## Benefits of IR
The use of IR in the Rosa brings several benefits:

1. **Platform Independence:** The IR allows executables to be developed and distributed as binaries that can run on multiple platforms without requiring recompilation or modification of the source code.
2. **Optimization Opportunities:** The IR provides a higher-level representation that enables verious optimization techniques. Loaders or interpreters can perform optimizations on the IR instructions.
3. **Efficient Execution:** The IR allows for efficient execution of the executable by being optimized for the specific machine on which it runs. During the loading or interpretation phase, the loader or interpreter analyzes the target platform's characteristics and translates the IR instructions into the appropriate machine code or virtual machine instructions specific to that platform.

## Usage of Intermediate Representation (IR) in Rosa
The IR in the Rosa file format is stored within the Code Section of the binary file.
The Code Section contains a sequence of instructions encoded in the Rosa language,
representing the platform-independent and architecture-agnostic intermediate representation of the executable code.

The IR instructions in the Code Section are designed to be agnostic of any specific
platform or architecture. They are expressed in a format that enables efficient 
interpretation or translation to the target platform and architecture during runtime.

Developers working with the Rosa file format can leverage the IR instructions to achieve
platform independence, enable optimization opportunities, ensure secure distribution, and
facilitate efficient execution of the executables.

## Example

```c
#include <stdio.h>

int main(void) {
    int a = 5;
    int *ptr = &a;
    *ptr = 10;

    printf("The value of a is %d\n", a);

    return 0;
}
```

The C code translated into the platform-independent and architecture-agnostic
intermediate representation (IR) used in the Rosa. Here's an example how the IR
instructions *may* look like:

```asm
main:
    LOAD_CONST R1, 5
    STORE a, R1
    LOAD_ADDR R2, a
    STORE ptr, R2
    LOAD ptr, R2
    LOAD_R R3, R2
    LOAD_CONST R4, 10
    STORE_R R3, R4
    LOAD a, R1
    PRINT R1
    LOAD_CONST R1, 0
    RETURN R1
```

And, on aarch64 systems, this IR will translated into:
```armasm
.text
main:
    SUB SP, SP, #12 // Reserve 12 bytes for 'a' (4) + 'ptr' (8)

    // int a = 5
    MOV W0, #5
    STR W0, [SP] // Store 'a' on the stack

    // Load the address of 'a' into 'ptr'
    LDR X0, =SP
    ADD X0, X0, #4
    STR X0, [SP, #8] // Store 'ptr' on the stack

    // Store the value 10 at the address stored in 'ptr'
    LDR X0, [SP, #4] // Load 'ptr' into register
    MOV W1, #10
    STR W1, [X0] // Store value 10 at the address in 'ptr'

    // Prepare the arguments and call printf
    LDR X0, =FORMAT // Address of the format string
    LDR X1, [SP] // The value of 'a'
    MOV X8, #0
    BL printf

    // Clean up the stack
    ADD SP, SP, #12

    // Return 0
    MOV W0, #0
    RET

.data
FORMAT: .asciz "The value of a is %d\n"
```

And, in amd64:
```x86asm
section .text
main:
    ; Reserve 12 bytes for 'a' (4) + 'ptr' (8)
    SUB RSP, 12

    ; int a = 5
    MOV DWORD [RSP], 5

    ; Load the address of 'a' into 'ptr'
    LEA RAX, [RSP]
    ADD RAX, 4
    MOV QWORD [RSP + 8], RAX

    ; Store the value 10 at the address stored in 'ptr'
    MOV RAX, QWORD [RSP + 8]
    MOV DWORD [RAX], 10

    ; Prepare the arguments and call printf
    MOV RDI, FORMAT
    MOV RSI, DWORD [RSP]
    CALL printf

    ADD RSP, 12 ; Clean up the stack

    MOV RAX, 0
    RET

section .data
FORMAT: db "The value of a is %d", 10, 0
```
