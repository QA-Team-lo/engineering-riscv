### **指南：在 RevyOS 上构建 OOFEM**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `OOFEM` 软件的说明。

#### 先决条件

可以工作的 C 工具链 (`gcc`)、`git` 和 `cmake` 是必须的

#### 步骤 1: 安装依赖并获取源代码

从 `OOFEM` 的官网获取源代码

```bash
$ sudo apt-get install build-essential xorg-dev git cmake gcc
$ wget https://www.oofem.org/cgi-bin/OOFEM/download.cgi?download=oofem-2.5.zip
$ mv 'download.cgi?download=oofem-2.5.zip' oofem.zip
$ unzip oofem.zip
```

#### 步骤 2: 使用 CMake 配置构建

执行:

```
$ mkdir build && cd build && cmake .. && make -j"$(nproc)"
```

即可完成构建，执行

```
./oofem
```

运行程序:

```
debian@revyos-lpi4a:~/oofem-2.5/build$ ./oofem

Options:

  -v  prints oofem version
  -f  (string) input file name
  -r  (int) restarts analysis from given step
  -ar (int) restarts adaptive analysis from given step
  -l  (int) sets treshold for log messages (Errors=0, Warnings=1,
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

#### 可用性测试

在 `./build` 目录下执行:

```
$ ctest
```

结果为:

```

99% tests passed, 1 tests failed out of 271

Total Test time (real) = 1717.85 sec

The following tests FAILED:
        144 - test_quasicontinuum3d.in (Failed)
Errors while running CTest
Output from these tests are in: /home/debian/oofem-2.5/build/Testing/Temporary/LastTest.log
Use "--rerun-failed --output-on-failure" to re-run the failed cases verbosely.
```

证明 `OOFEM` 在 RevyOS 上具有基本可用性
