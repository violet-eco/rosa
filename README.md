# Rosa

Rosa is a specialized binary file format designed for distributing
applications. It offers platform independence, architecture agnosticism
and efficient execution through the use of an intermediate representation
and runtime interpretation or translation.

## Features
- **Platform Independence:** Rosa binaries can be distributed on every platform without the need for recompilation.
- **Architecture Agnosticism:** Rosa is designed to be architecture-agnostic, enabling binaries to execute efficiently on various hardware architectures. The indermediate representation ensure optimal execution on each target architecture.
- **Secure Distribution:** The use of IR within the Rosa helps protect the source code of closed-source applications. As the IR consists of basic instructions and is not the representative of the original source code, it provides a level of abstraction that makes reverse engineering more challenging.
- **Efficient Execution:** Rosa allows for efficient execution by being optimized for the specific machine on which they run. During the loading or interpretation phase, the loader or interpreter can analyze the target platform's characteristics and translate the IR instructions into appropriate machine code or virtual machine instructions specific to that platform.

## Getting Started
> The commands below are purely stubs, may be subject to future changes or may not be available at this time.

### Prerequisites
- Rust (version 1.69+)

### Installation
1. Install the project with Cargo:  
```sh
cargo install https://github.com/gulje/rosa.git
```
2. Now, you should be able use the `rosa` command on your shell. Run the `rosa -v` command to verify the installation.

### Usage
1. Create a project in your desired programming language.
2. Compile the project into the intermediate representation using the available tools or libraries.
3. Use the compiler to generate a Rosa binary:  
```sh
rosa compile -i input.ir -o binary.rosa
```
4. Distribute the generated Rosa binary for users to run on their respective platforms.
5. Users can execute the binary on their platform using the Rosa interpreter or translator:  
```sh
rosa run -f binary.rosa
```
or
```sh
rosa translate -f binary.rosa -a x86_64
```

# License
The Rosa project is licensed under the Apache-2.0 License. See the LICENSE
for more details.
