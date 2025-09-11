### **Guide: Running Code-Aster on RevyOS**

This document provides instructions for building the `Code-Aster` software from source on a RISC-V device running RevyOS.

### Summary

At present, `Code-Aster` cannot be successfully compiled on Debian/RevyOS-based RISC-V platforms.

### Reproduction Steps

#### Obtain the Source

```bash
$ wget https://code-aster.org/FICHIERS/aster-full-src-14.6.0-1.noarch.tar.gz
$ tar -vxzf aster-full-src-14.6.0-1.noarch.tar.gz
```

#### Build

```bash
python3 setup.py
```

#### Log

```bash
Installation on :
Debian GNU/Linux trixie/sid

Linux revyos-lpi4a 6.6.92-th1520 #2025.05.26.14.02+c9a17b235 SMP Mon May 26 14:22:33 UTC 2025 riscv64

Checking for max command length...   32768.0
Checking for file... /usr/bin/file
Checking for ar... no
Checking for architecture... Linux / posix / x86_64
Checking for number of processors (core)... 4 (will use: make -j 4)
Checking for Code_Aster platform type... LINUX64
Checking for bash... no
Checking for ksh... no
Checking for zsh... no
Checking for Python version... 3.11.4
Checking for python3.11... no
Checking for libpython3.11.so... no
Checking for libpython3.11.a... no
Checking for libpython3.11m... no
Checking for libpython3.11m.so... no
Checking for libpython3.11m.a... no
Unable to find python3.11 or libpython3.11.so or libpython3.11.a or python3.11m or libpython3.11m.so or libpython3.11m.a
```

The build system fails.

### Root Cause Analysis

Known issues in the `Code_Aster` build system include:

* Calls to the deprecated `python2` module
* Hard-coded architecture as "x86-64" with no fallback for unknown architectures
* System library search restricted to `/usr/lib/x86_64-linux-gnu`, which prevents finding libraries on RISC-V
* Python library handling uses `sys.version[:3]`
  * On Python 3.11+, this returns `3.1` instead of `3.11`, leading to incorrect version strings
