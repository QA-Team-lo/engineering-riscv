### **指南：在 RevyOS 上运行 FElt**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `FElt` 软件的说明。

### 总结

目前 `FElt` 无法在基于 Debian/RevyOS 的 RISC-V 平台上正常编译。

### 复现步骤

#### 安装依赖
```
sudo apt-get install -y \
  build-essential gcc gfortran \
  libx11-dev libxaw7-dev libxt-dev libxmu-dev libxpm-dev bison flex
```

#### 编译
```
make
```

#### 编译结果
```
(忽略前面有关 string.h 和 stdlib.h 的报错，加上头文件即可解决)
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

#### 原因分析

FElt(2009) 用的 Lex/Bison 和 C 写法太老，gcc 14.2.0, bison 3.8.2, flex 2.6.4 已无法正常编译。

#### RISC-V 适配价值

FElt 上一次更新在 2013 年，距今已经十年多，且官方下载渠道基本已无人下载，论坛/邮件列表自 2011 年后也无人活跃。可能没有太多适配价值。
