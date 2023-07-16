# Code Section Specification

## Introduction
The Code Section is a critical component of the Rosa. It contains
the optimized executable code of the program, encoded in the low-level
Rosa language. This documentation provides an understanding of the structure,
instructions and specifications of the Code Section.

## Structure
The Code Section has the following structure:
```rs
struct CodeSection {
    size: [u8, 4],
    instructions: Vec<Instruction>,
}
```

### Size
The size field is a 4-byte value that represents the size of the code section
in bytes, including the size field itself.

- Size field is a 4-byte value
- Loaders and interpreters must use the size field to correctly locate and parse
the Code Section

### Instructions
> Refer to [this document](./ir.md) for further information about intermediate representation.

The instructions field holds the platform-independent and architecture-agnostic
intermediate representation (IR) instructions. These instructions are not specific
to any particular platform or architecture but can be interpreted or translated
to the target platform and architecture at runtime.

## Instruction Format
The instructions are represented using a platform-independent and architecture-agnostic
intermediate representation. Each instruction consists of the following components:
```rs
struct Instructon {
    opcode: u8,
    operands: Vec<Operand>,
}
```

### Opcode
The opcode field represents the operation or action to be performed by the instruction.
It is encoded as a single byte and specifies the operation or behaviour to be executed.

### Operands
The operands field provides additional data or parameters required by the instruction.
The number, size, and format of operands can vary on the specific opcode. Operands can represent constants, memory addresses, registers, or other data required for the
instruction's execution.

- **Register Operand:** A register operand identifies a specific register used in the instruction. It is represented as a numeric value.
- **Immediate Operand:** An immediate operand represents a constant value that is embedded directly in the instruction. It is encoded as a numeric value.
- **Memory Operand:** A memory operand specifies a memory address or location accessed by the instruction. It may include information such as the base address, offset, and access size.
- **Label Operand:** A label operand represents a symbolic reference to another instruction or data location within the Code Section.

## Conclusion
The Code Section of the Rosa file stores the platform-independent and architecture-agnostic intermediate representation (IR) instructions
of the executable code. The use of IR allows for platform and architecture
agnosticism, enabling the interpretation or translation of the code to
the target platform and architecture at runtime. By following the specifications
outlined here, developers can correctly interpret or translate the Code Section
instructions, faciliating the execution on different platforms and architectures.