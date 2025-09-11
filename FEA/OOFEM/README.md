### **Guide: Building OOFEM on RevyOS**

This document provides instructions for building the `OOFEM` software from source on a RISC-V device running RevyOS.


#### **Prerequisites**

A working C toolchain (`gcc`), along with `git` and `cmake`, is required.

#### **Step 1: Install Dependencies and Retrieve the Source Code**

Obtain the source code from the official `OOFEM` website:

```bash
$ sudo apt-get install build-essential xorg-dev git cmake gcc
$ wget https://www.oofem.org/cgi-bin/OOFEM/download.cgi?download=oofem-2.5.zip
$ mv 'download.cgi?download=oofem-2.5.zip' oofem.zip
$ unzip oofem.zip
```

#### **Step 2: Configure and Build with CMake**

Run the following:

```bash
$ mkdir build && cd build && cmake .. && make -j"$(nproc)"
```

Once the build is complete, run:

```bash
./oofem
```

Example program output:

```text
debian@revyos-lpi4a:~/oofem-2.5/build$ ./oofem

Options:

  -v  prints oofem version
  -f  (string) input file name
  -r  (int) restarts analysis from given step
  -ar (int) restarts adaptive analysis from given step
  -l  (int) sets threshold for log messages (Errors=0, Warnings=1,
            Relevant=2, Info=3, Debug=4)
  -rn turns on renumbering
  -qo (string) redirects the standard output stream to given file
  -qe (string) redirects the standard error stream to given file
  -c  creates context file for each solution step


Copyright (C) 1994-2017 Borek Patzak
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

debian@revyos-lpi4a:~/oofem-2.5/build$ ./oofem -v

OOFEM version 2.5 (riscv64-Linux, fm;tm;sm)
of Sep  9 2025 on revyos-lpi4a

Copyright (C) 1994-2017 Borek Patzak
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

#### **Availability Test**

In the `./build` directory, run:

```bash
$ ctest
```

Result:

```text

99% tests passed, 1 tests failed out of 271

Total Test time (real) = 1717.85 sec

The following tests FAILED:
        144 - test_quasicontinuum3d.in (Failed)
Errors while running CTest
Output from these tests are in: /home/debian/oofem-2.5/build/Testing/Temporary/LastTest.log
Use "--rerun-failed --output-on-failure" to re-run the failed cases verbosely.
```

This demonstrates that `OOFEM` has basic functionality on RevyOS.
