# Header Section Specification

## Introduction
The Header Section is crucial component of Rosa files. It is located
at the beginning of the file and contains essential metadata and configuration
about the binary. 

## Structure
The Header Section has the following structure:

```rs
struct RosaHeader {
    pub signature: [u8; 4],             // A unique identifier indicating the file format ("ROSA")
    pub version: [u8; 3],               // Version number of Rosa (MAJOR.MINOR.PATCH)
    pub optimization: RosaOptimization  // Includes different levels of optimization
    pub flags: RosaFlags,               // Additional flags or attributes
}

enum RosaOptimization {
    None,
    Level1,
    Level2,
    Level3,
}

enum RosaFlags {
    DEBUG_INFO      = 0b00000001;
    SECURE_EXEC     = 0b00000010;
    COMPATIBILITY   = 0b00000100;
    // ...
}
```

### Signature
The Signature field is a 4-byte sequence that serves as a unique
identifier of the Rosa file. It provides a means for loaders and
interpreters to verify the file's format. The Signature is represented
as `ROSA` string.

- The Signature field must be 4 bytes in size.
- Loaders and interpreters must compare the Signature against the
`ROSA` value to verify the file's format.

### Version
The Version field represents the version number of the Rosa. It consists
of three bytes: major, minor and patch.

The major version indicates the incompatible changes, the minor version
represents backwards-compatible additions, and the patch version signifies
backward-compatible bug fixes.

- The Version field must be 3 bytes in size, representing the major,
minor and patch versions.
- Each version component is an 8-bit unsigned integer ranging from 0 to 255.
- The version numbers follow the [Semantic Versioning](https://semver.org/) scheme.
- Loaders and interpreters use the Version field to adapt their behaviour based
on the version of the Rosa file.

### Optimization Level
The Optimization Level field specifies the level of optimization applied
during the compilation process. It is a single byte (u8) that represents the optimization level using an enumeration.

- Loaders and interpreters can adjust their execution strategies based on the optimization level specified.

Valid values:
- `None` (`0`): No optimization is applied. The resulting binary may have larger code size and may not be as efficient in terms of execution speed.
- `Level1` (`1`): Basic level of optimization is applied. The loader or interpreter performs simple optimizations, such as _constant folding_ and _dead code elimination_, to improve code efficiency.
- `Level2` (`2`): Intermeditate level of optimization is applied. In addition to the optimizations performed in the `Level1`, more advanced techniques are applied, such as _loop optimizations_, _function inlining_, and _register allocation_.
- `Level3` (`3`): Aggressive level of optimization is applied.  In addition to the optimizations performed in `Level2`, further advanced techniques are applied, including _aggressive loop transformations_ and _vectorization_. This level aims for maximum performance but may result in longer compilation times.

### Flags
The Flags field is a 2-byte value that indicates additional attributes or features 
associated with the binary. The individual bits within the Flags field represent
specific flags or attributes. Each bit can be set (1) or unset (0) to enable or disable
a particular feature. The meaning and usage of each flag bit are defined by the Rosa
file format specification.

- The Flags field is 2 bytes in size.
- Each bit within the Flags field represents a specific flag or attribute.
- The meaning and usage of each flag bit are defined in the Rosa file format specification.
- Loaders and interpreters must interpret the flag bits to determine the binary's
attributes and adapt their behavior accordingly.

Values:
- Flag 1: Debugging Information
    - When set, indicates the presnece of additional debugging information in the binary.
    - Loaders or interpreters can use this flag to enable debugging features.
- Flag 2: Secure Execution
    - Indicates that the binary was compiled with security measures enabled.
    - Loaders or interpreters can enforce additional security checks or sandboxing mechanisms for such binaries.
- Flag 3: Compatibility Mode
    - Specifies a specific compatibility mode for the binary.
    - Loaders or interpreters can adjust their behaviour to handle compatibility issues with older versions.

## Conclusion
The Header Section is a vital component of Rosa files, providing
essential metadata and configuration information. This documentation
has provided a comprehensive understanding of the structure, fields
and specifications of the Header Section. By adhering to the specifications
outlined here, developers can parse and interpret the Header Section,
enabling correct and efficient handling of Rosa binary files.