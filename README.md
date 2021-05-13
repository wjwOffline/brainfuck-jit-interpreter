# brainfuck-jit-interpreter
A handwritten Brainfuck JIT interpreter, mainly used for demonstration purpose.

### How to use?

\* Please make sure you have installed `python` 3.75 or above and `clang++`.

```
make && ./interpreter ./bfs/HELLO_WORLD.bf [--jit]
```

### Limitations of this program:

* No exception-handling support.
* No thread-safe guaranteed.
* No fine-tuning of the generated assembly code.
* The amount of consecutive "+", "-", "<" and ">" showing up in the source code cannot exceed 255, for "]", cannot exceed 23.
* Only support X86-64 on macOS or Linux, but the compilation is highly dependent on the version of the compilers. **Some other optimization levels may fail** (unknown reason).

### Benchmark

```text
[System] macOS 10.15.7
[Compiler] Apple clang version 12.0.0 (clang-1200.0.32.29)
[Optimization Level] -O0

Benchmark for 10 seconds: (higher score is better)
    8939 .out_interpret (win)
    6438 .out_jit
```

```text
[System] CentOS Linux release 8.3.2011
[Compiler] clang version 10.0.1 (Red Hat 10.0.1-1.module_el8.3.0+467+cb298d5b)
[Optimization Level] -O0

Benchmark for 10 seconds: (higher score is better)
   6202 .out_interpret
   6483 .out_jit (win)

```

### Sidenote

***The actual performance of the interpreter that compiled by higher level optimization flags (-01 / -02) will be at least 1 time higher than the performance of the JIT compiler***. 

Because we don't have too much fine-tuning about the machine code generated by the JIT compiler, for example, too much emitted `jmp` may suffer the program from branch prediction penalty, no execution flow analysis may cause the JIT compiler emits more instructions than the program really need, etc. But in the contrast, the advanced C/C++ compilers are really good at generating highly optimized machine code even the program may suffer from small wrong branch prediction penalty. So, whether the performance of the JIT compiler will be better than the interpreter, it depends on the real implementation :)
