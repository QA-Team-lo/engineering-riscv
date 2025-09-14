### **指南：在 RevyOS 上运行 Code-Aster**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `Code-Aster` 软件的说明。

### 总结

目前 `Code-Aster` 无法在基于 Debian/RevyOS 的 RISC-V 平台上正常编译。

### 复现步骤

#### 获取源码
```
$ wget https://code-aster.org/FICHIERS/aster-full-src-14.6.0-1.noarch.tar.gz
$ tar -vxzf aster-full-src-14.6.0-1.noarch.tar.gz
```

#### 编译
```
python3 setup.py
```

#### 日志
```bash
Installation on :
Debian GNU/Linux trixie/sid

Linux revyos-lpi4a 6.6.92-th1520 #2025.05.26.14.02+c9a17b235 SMP Mon May 26 14:22:33 UTC 2025 riscv64

Checking for max command length...   32768.0
Checking for file... /usr/bin/file
Checking for ar... no
Checking for architecture... Linux / posix / x86\_64
Checking for number of processors (core)... 4 (will use: make -j 4)
Checking for Code\_Aster platform type... LINUX64
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

构建系统运行失败

#### 原因分析

`Code_Aster` 构建系统已知的问题有：

- 调用了 `python2` 模块
- 硬编码架构为 "x86-64"，且不对未知架构放行
- 系统库目录只检测 `/usr/lib/x86_64-linux-gnu`, 导致 RISC-V 架构下找不到
- Python 库相关的操作，使用 `sys.version[:3]`
  - 导致 3.11+ 实际得到的是 3.1，最终拼错版本字符串
