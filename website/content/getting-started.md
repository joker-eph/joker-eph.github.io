---
title: "Getting Started"
date: 2019-11-13T09:01:53-08:00
draft: false
---

The following instructions for compiling and testing MLIR assume that you have
`git`, [`ninja`](https://ninja-build.org/), and a working C++ toolchain. In the
future, we aim to align on the same level of platform support as
[LLVM](https://llvm.org/docs/GettingStarted.html#requirements). For now, MLIR
has been tested on Linux and macOS, with recent versions of clang and with
gcc 7.

```sh
git clone https://github.com/llvm/llvm-project.git
git clone https://github.com/tensorflow/mlir llvm-project/llvm/projects/mlir
mkdir llvm-project/build
cd llvm-project/build
cmake -G Ninja ../llvm -DLLVM_BUILD_EXAMPLES=ON -DLLVM_TARGETS_TO_BUILD="host"
cmake --build . --target check-mlir
```

To compile and test on Windows using Visual Studio 2017:

```bat
REM In shell with Visual Studio environment set up, e.g., with command such as
REM   $visual-studio-install\Auxiliary\Build\vcvarsall.bat" x64
REM invoked.
git clone https://github.com/llvm/llvm-project.git
git clone https://github.com/tensorflow/mlir llvm-project\llvm\projects\mlir
mkdir llvm-project\build
cd llvm-project\build
cmake ..\llvm -G "Visual Studio 15 2017 Win64" -DLLVM_BUILD_EXAMPLES=ON -DLLVM_TARGETS_TO_BUILD="host" -DCMAKE_BUILD_TYPE=Release -Thost=x64
cmake --build . --target check-mlir
```

As a starter, you may try [the tutorial](g3doc/Tutorials/Toy/Ch-1.md) on
building a compiler for a Toy language.
