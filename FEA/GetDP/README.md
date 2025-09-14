### **Guide: Building GetDP on RevyOS**

This document provides instructions for building GetDP from source on a RISC-V device running the RevyOS Debian derivative.

#### **Prerequisites**

A working C++ toolchain (`g++`), along with `git` and `cmake`, are required.

#### **Step 1: Retrieve the Source Code**

Clone the GetDP repository:

```bash
$ git clone https://gitlab.onelab.info/getdp/getdp.git
```

#### **Step 2: Configure and Build with CMake**

Create a build directory, run CMake, and compile using all available cores:

```bash
$ mkdir build && cd build && cmake .. && make -j"$(nproc)"
```

Once the build completes, run the executable:

```bash
$ ./getdp
```

You should see output similar to:

```text
debian@revyos-lpi4a:~/getdp-git-source/build$ ./getdp

  -res file(s)              load processing results from file(s)
  -name string              use string as generic file name
  -adapt file               read adaptation constraints from file
  -order num                restrict maximum interpolation order
  -cache                    cache network computations to disk
Linear solver options:
  -solver file              specify parameter file (default: solver.par)
  -'Parameter' num          override value of solver parameter 'Parameter'
Output options:
  -bin                      create binary output files
  -v2                       generate mesh-based Gmsh output files when possible
Other options:
  -check                    interactive check of problem structure
  -v num                    set verbosity level (default: 5)
  -cpu                      report CPU times for all operations
  -p num                    set progress indicator update interval (default: 10)
  -onelab name [address]    communicate with ONELAB (file or server address)
  -setnumber name value     set constant number (name=value) (or -sn)
  -setstring name value     set constant string (name=value) (or -ss)
  -version                  display version number
  -info                     display detailed version information
  -help                     show this help message
```

#### **Availability Test**

Running the magnetostatics example:

```
debian@revyos-lpi4a:~/getdp-git-source/build$ make test
Running tests...
Test project /home/debian/getdp-git-source/build
    Start 1: ../examples/magnet.pro
1/1 Test #1: ../examples/magnet.pro ...........   Passed    0.81 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.81 sec
```

This demonstrates that `getdp` is basically functional on RevyOS.
