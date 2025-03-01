---
layout: docs
toc_group: llvm
link_title: LLVM Languages Reference
permalink: /reference-manual/llvm/
---
# GraalVM LLVM Runtime

The GraalVM LLVM runtime can execute programming languages that can be transformed to LLVM bitcode.
This includes languages like C/C++, Fortran and others.

In contrast to static compilation that is normally used for LLVM-based languages, GraalVM's implementation of the `lli` tool first interprets LLVM bitcode and then dynamically compiles the hot parts of the program using the Graal compiler.
This allows seamless interoperability with the dynamic languages supported by GraalVM.

## Getting Started

As of GraalVM for JDK 21, the GraalVM LLVM runtime is available as a standalone distribution. 
You can download a standalone based on Oracle GraalVM or GraalVM Community Edition. 

1. Download the LLVM standalone for your operating system:
   - Native standalone
      * [Linux x64](https://gds.oracle.com/api/20220101/artifacts/05013E46CB6993C6E0631818000A2314/content)
      * [Linux AArch64](https://gds.oracle.com/api/20220101/artifacts/04F488A0621A4081E0631818000A781E/content)
      * [macOS x64](https://gds.oracle.com/api/20220101/artifacts/04F488A0621F4081E0631818000A781E/content)
      * [macOS AArch64](https://gds.oracle.com/api/20220101/artifacts/04F488A062244081E0631818000A781E/content)
      * [Windows x64](https://gds.oracle.com/api/20220101/artifacts/04F488A062294081E0631818000A781E/content)
   - JVM standalone
      * [Linux x64](https://gds.oracle.com/api/20220101/artifacts/04F488A0622E4081E0631818000A781E/content)
      * [Linux AArch64](https://gds.oracle.com/api/20220101/artifacts/05013E46CB5D93C6E0631818000A2314/content)
      * [macOS x64](https://gds.oracle.com/api/20220101/artifacts/04F488A062384081E0631818000A781E/content)
      * [macOS AArch64](https://gds.oracle.com/api/20220101/artifacts/04F488A0623D4081E0631818000A781E/content)
      * [Windows x64](https://gds.oracle.com/api/20220101/artifacts/04F556B005683A58E0631818000A322D/content)

2. Unzip the archive:

    > Note: If you are using macOS Catalina and later you may need to remove the quarantine attribute:
    ```shell
    sudo xattr -r -d com.apple.quarantine <archive>.tar.gz
    ```
    
    Extact:
    ```shell
    tar -xzf <archive>.tar.gz
    ```

3. A standalone comes with a JVM in addition to its native launcher. Check the version to see GraalVM LLVM runtime is active:
    ```shell
    ./path/to/bin/lli --version
    ```

Now you can execute programs in the LLVM bitcode format.

### LLVM Toolchain

Additionally, a prebuilt LLVM toolchain is bundled with the GraalVM LLVM runtime.

1. Get the location of the toolchain, using the `--print-toolchain-path` argument of `lli`:
    ```shell
    ./path/to/bin/lli --print-toolchain-path
    ```

2. Set the `LLVM_TOOLCHAIN` environment variable: 
    ```shell
    export LLVM_TOOLCHAIN=$(./path/to/bin/lli --print-toolchain-path)
    ```

3. Then see the content of the toolchain path for a list of available tools:
    ```shell
    ls $LLVM_TOOLCHAIN
    ```

Now you can compile C/C++ code to LLVM bitcode using `clang` shipped with GraalVM via the LLVM toolchain.

## Run LLVM Bitcode on GraalVM

To run LLVM-based languages on GraalVM, the binaries need to be compiled with embedded bitcode.
The [Compiling](Compiling.md) guide provides information on how to compile a program to LLVM bitcode and what file format is expected.

The syntax to execute programs in LLVM bitcode format on GraalVM is:
```shell
lli [LLI options] [GraalVM options] [polyglot options] <bitcode file> [program args]
```

Here, `<bitcode file>` is [a compiled program with embedded LLVM bitcode](Compiling.md).
See [LLI Command Options](Options.md) or use `lli --help` for options explanations.

For example, put this C code into a file named `hello.c`:
```c
#include <stdio.h>

int main() {
    printf("Hello from GraalVM!\n");
    return 0;
}
```

Then compile `hello.c` to an executable `hello` with embedded LLVM bitcode and run it as follows:
```shell
$LLVM_TOOLCHAIN/clang hello.c -o hello
lli hello
```

Note: LLVM bitcode is platform-dependent.
The program must be compiled to bitcode for an appropriate platform.

## Further Reading

- [LLVM Compatibility](Compatibility.md)
- [Compiling to LLVM Bitcode](Compiling.md)
- [Debugging on the GraalVM LLVM Runtime](Debugging.md)
- [Interoperability with Other Languages](Interoperability.md)
- [Interaction of GraalVM with Native Code](NativeExecution.md)
- [LLI Command Options](Options.md)