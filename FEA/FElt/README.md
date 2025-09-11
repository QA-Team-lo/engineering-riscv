### **Guide: Running FElt on RevyOS**

This document provides instructions for building the `FElt` software from source on a RISC-V device running RevyOS.

---

### **Summary**

At present, `FElt` cannot be successfully compiled on Debian/RevyOS-based RISC-V platforms.

---

### **Reproduction Steps**

#### Install dependencies

```bash
sudo apt-get install -y \
  build-essential gcc gfortran \
  libx11-dev libxaw7-dev libxt-dev libxmu-dev libxpm-dev bison flex
```

#### Compile

```bash
make
```

#### Compilation result

```text
(ignore the earlier errors related to string.h and stdlib.h, which can be fixed by adding the headers)
Making all in ./lib/Matrix ...
make[2]: Entering directory '/home/debian/felt-3.07/lib/Matrix'
make[2]: Nothing to be done for 'all'.
make[2]: Leaving directory '/home/debian/felt-3.07/lib/Matrix'
Making all in ./lib/Widgets ...
make[2]: Entering directory '/home/debian/felt-3.07/lib/Widgets'
gcc -O3 -I/usr/include/X11 -I../../include   -c -o laygram.o laygram.c
laygram.y:254:1: error: return type defaults to ‘int’ [-Wimplicit-int]
laygram.y:259:1: error: return type defaults to ‘int’ [-Wimplicit-int]
y.tab.c: In function ‘LayYYparse’:
y.tab.c:272:26: error: implicit declaration of function ‘LayYYlex’ [-Wimplicit-function-declaration]

y.tab.c:313:5:? [-Wimplicit-function-declaration]
make[2]: *** [<builtin>: laygram.o] Error 1
make[2]: Leaving directory '/home/debian/felt-3.07/lib/Widgets'
make[1]: *** [../etc/Makefile.dirs:24: all] Error 1
make[1]: Leaving directory '/home/debian/felt-3.07/lib'
make: *** [etc/Makefile.dirs:24: all] Error 1
```

---

### **Cause Analysis**

FElt (2009) uses very old Lex/Bison and C syntax that is no longer compatible with modern toolchains:

* **gcc 14.2.0**
* **bison 3.8.2**
* **flex 2.6.4**

As a result, the code fails to compile on current systems.
