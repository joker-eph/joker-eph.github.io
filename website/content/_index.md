---
title: "Multi-Level Intermediate Representation Overview"
date: 2019-11-13T09:01:53-08:00
draft: false
---

The MLIR project aims to define a common intermediate representation (IR) that
will unify the infrastructure required to execute high performance machine
learning models in TensorFlow and similar ML frameworks. This project will
include the application of HPC techniques, along with integration of search
algorithms like reinforcement learning. This project aims to reduce the cost to
bring up new hardware, and improve usability for existing TensorFlow users.

Note that this repository contains the core of the MLIR framework. The
TensorFlow compilers we are building on top of MLIR will be part of the
main TensorFlow repository soon.

## What is MLIR for?

MLIR is intended to be a hybrid IR which can support multiple different
requirements in a unified infrastructure. For example, this includes:

*   The ability to represent all TensorFlow graphs, including dynamic shapes,
    the user-extensible op ecosystem, TensorFlow variables, etc.
*   Optimizations and transformations typically done on a TensorFlow graph, e.g.
    in Grappler.
*   Quantization and other graph transformations done on a TensorFlow graph or
    the TF Lite representation.
*   Representation of kernels for ML operations in a form suitable for
    optimization.
*   Ability to host high-performance-computing-style loop optimizations across
    kernels (fusion, loop interchange, tiling, etc) and to transform memory
    layouts of data.
*   Code generation "lowering" transformations such as DMA insertion, explicit
    cache management, memory tiling, and vectorization for 1D and 2D register
    architectures.
*   Ability to represent target-specific operations, e.g. the MXU on TPUs.

MLIR is a common IR that also supports hardware specific operations. Thus,
any investment into the infrastructure surrounding MLIR (e.g. the compiler
passes that work on it) should yield good returns; many targets can use that
infrastructure and will benefit from it.

MLIR is a powerful representation, but it also has non-goals. We do not try to
support low level machine code generation algorithms (like register allocation
and instruction scheduling). They are a better fit for lower level optimizers
(such as LLVM). Also, we do not intend MLIR to be a source language that
end-users would themselves write kernels in (analogous to CUDA C++). While we
would love to see a kernel language happen someday, that will be an independent
project that compiles down to MLIR.

## Compiler infrastructure

We benefited from experience gained from building other IRs (HLO, LLVM and SIL)
when building MLIR. We will directly adopt existing best practices, e.g. writing
and maintaining an IR spec, building an IR verifier, providing the ability to
dump and parse MLIR files to text, writing extensive unit tests with the
[FileCheck](https://llvm.org/docs/CommandGuide/FileCheck.html) tool, and
building the infrastructure as a set of modular libraries that can be combined
in new ways. We plan to use the infrastructure developed by the XLA team for
performance analysis and benchmarking.

Other lessons have been incorporated and integrated into the design in subtle
ways. For example, LLVM has non-obvious design mistakes that prevent a
multithreaded compiler from working on multiple functions in an LLVM module at
the same time. MLIR solves these problems by having per-function constant pools
and by making references explicit with `function_ref`.
