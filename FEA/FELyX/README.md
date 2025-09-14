### **Guide: Building FELyX on RevyOS**

This document provides instructions for building the `FELyX` software from source on a RISC-V device running RevyOS.


### **Summary**

At present, `FELyX` cannot be successfully compiled on Debian/RevyOS-based RISC-V platforms.

### **Reproduction Steps**

#### **Step 1: Retrieve the Source Code**

Download the source code from the official `FELyX` website:
[https://felyx.sourceforge.net/](https://felyx.sourceforge.net/)

#### **Step 2: Update the Build System and Compile**

Since the `FELyX` build system is outdated and does not recognize the `riscv64` architecture, two files must be updated manually:

```bash
$ wget -O config.guess https://git.savannah.gnu.org/cgit/config.git/plain/config.guess
$ wget -O config.sub   https://git.savannah.gnu.org/cgit/config.git/plain/config.sub
$ chmod +x config.guess config.sub
$ CXXFLAGS="-std=gnu++03 -fpermissive" ./configure
```

Then compile:

```bash
make V=1
```

#### **Compilation Result**

```text
Making all in src
make[1]: Entering directory '/home/debian/felyx/src'
make  all-recursive
make[2]: Entering directory '/home/debian/felyx/src'
Making all in mtl
make[3]: Entering directory '/home/debian/felyx/src/mtl'
Making all in mtl-specializations
make[4]: Entering directory '/home/debian/felyx/src/mtl/mtl-specializations'
g++ -DHAVE_CONFIG_H -I. -I../../../src -I../../../src -I../../../src/mtl/mtl-specializations    -std=gnu++03 -fpermissive -MT mtl_specializations.o -MD -MP -MF .deps/mtl_specializations.Tpo -c -o mtl_specializations.o mtl_specializations.cc

In file included from ../../../src/mtl/dense1D.h:36,
                 from ../../../src/mtl/mtl.h:47,
                 from mtl_specializations.cc:10:
../../../src/mtl/reverse_iter.h: In member function ‘:tl::reverse_iter<Iter>::difference_type mtl::reverse_iter<Iter>::index() const’
../../../src/mtl/reverse_iter.h:71:16: error: ‘current’ was not declared in this scope
   71 |     Iter tmp = current;
      |                ^~~~~~~

...
```

#### **Cause Analysis**

The code is incompatible with modern C++ standards. For example:

```diff
- Iter tmp = current;
+ Iter tmp = this->current;
```
The errors are caused by outdated C++ code patterns that are not accepted by modern compilers, requiring source code patches for successful compilation.
