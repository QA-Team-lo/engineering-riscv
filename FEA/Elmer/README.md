### **Guide: Building Elmer on RevyOS**

This document provides instructions for building the `Elmer` software from source on a RISC-V device running RevyOS.

#### **Prerequisites**

A working C toolchain (`gcc`), along with `git` and `cmake`, is required.

#### **Step 1: Install Dependencies and Retrieve the Source Code**

Clone the source code from the official `Elmer` GitHub repository:

```bash
$ git clone git@github.com:ElmerCSC/elmerfem.git
```

#### **Step 2: Configure and Build with CMake**

Run:

```bash
$ mkdir build && cd build && cmake .. && make -j"$(nproc)"
$ sudo make install
```

Once the build is complete, execute:

```bash
$ elmerf90
```

#### **Program Execution**

```bash
debian@revyos-lpi4a:~/elmerfem$ elmerf90 --help
no elmerice
/usr/bin/f95 --help -fallow-argument-mismatch -DCONTIG= -DHAVE_EXECUTECOMMANDLINE -DUSE_ISO_C_BINDINGS -DUSE_ARPACK -O2 -g -fPIC -shared -I/usr/local/share/elmersolver/include -L/usr/local/share/elmersolver/../../lib/elmersolver -shared -lelmersolver

  -std=<standard>          Assume that the input sources are for <standard>.
  --sysroot=<directory>    Use <directory> as the root directory for headers
                           and libraries.
  -B <directory>           Add <directory> to the compiler's search paths.
  -v                       Display the programs invoked by the compiler.
  -###                     Like -v but options quoted and commands not executed.
  -E                       Preprocess only; do not compile, assemble or link.
  -S                       Compile only; do not assemble or link.
  -c                       Compile and assemble, but do not link.
  -o <file>                Place the output into <file>.
  -pie                     Create a dynamically linked position independent
                           executable.
  -shared                  Create a shared library.
  -x <language>            Specify the language of the following input files.
                           Permissible languages include: c c++ assembler none
                           'none' means revert to the default behavior of
                           guessing the language based on the file's extension.

Options starting with -g, -f, -m, -O, -W, or --param are automatically
 passed on to the various sub-processes invoked by f95.  In order to pass
 other options on to these processes the -W<letter> options must be used.

For bug reporting instructions, please see:
<file:///usr/share/doc/gcc-14/README.Bugs>.
```

```bash
debian@revyos-lpi4a:~/elmerfem$ elmerf90 --version
no elmerice
/usr/bin/f95 --version -fallow-argument-mismatch -DCONTIG= -DHAVE_EXECUTECOMMANDLINE -DUSE_ISO_C_BINDINGS -DUSE_ARPACK -O2 -g -fPIC -shared -I/usr/local/share/elmersolver/include -L/usr/local/share/elmersolver/../../lib/elmersolver -shared -lelmersolver


GNU Fortran (Debian 14.2.0-11revyos1) 14.2.0
Copyright (C) 2024 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

This confirms that `Elmer` has **basic functionality** on RevyOS.
