# Rosa File Format Documentation

## Introduction
Rosa is a specialized binary format for distributing
programs across multiple platforms and architectures.
This comprehensive documentation provides an in-depth
understanding of the Rosa, including its structure,
header specifications, sections, instruction set,
memory layout and reloaction mechanisms.

## File Structure
A Rosa file consists of the following sections, with specific order:

### Header Section
> Please refer to the [header document](header.md) for specification documents.

The header section is located at the beginning of the file provides
crucial information about the binary. It has the following structure:

- Signature: A 4-byte value that serves as a unique identifier indicating
the file format.
- Version: A 3-byte array that represents the version number of the Rosa file.
    - The version numbers adhere to the Semantic Versioning specification, where:
      - The major version represents incompatible changes
      - The minor version represents backwards-compatible additions
      - The patch version represents backwards-compatible bug fixes
- Optimization: A 1-byte value that indicates the optimization level.
  - `None`: No optimization is applied.
  - `Level1`: Basic level of optimization is applied. 
  - `Level2`: Intermeditate level of optimization is applied.
  - `Level3`: Aggressive level of optimization is applied.
- Flags: A 1-byte value that indicates additional flags or attributes associated
with the binary. It provides information about various aspects of the binary
and guides the loader or interpreter's behaviour during execution.

### Code Section
> Please refer to the [code document](code.md) for specification documents.