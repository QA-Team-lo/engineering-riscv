### **指南：在 RevyOS 上构建 FELyX**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `FELyX` 软件的说明。

### 总结

目前 `FELyX` 无法在基于 Debian/RevyOS 的 RISC-V 平台上正常编译。

### 复现步骤

#### 步骤 1: 获取源代码

从 `FELyX` 的官网获取源代码: https://felyx.sourceforge.net/

#### 步骤 2: 更新构建系统并编译

由于 `FELyX` 的构建系统太老，无法识别到 `riscv64` 这个架构，故需要手动更新两个文件
```
$ wget -O config.guess https://git.savannah.gnu.org/cgit/config.git/plain/config.guess
$ wget -O config.sub   https://git.savannah.gnu.org/cgit/config.git/plain/config.sub
$ chmod +x config.guess config.sub
$ CXXFLAGS="-std=gnu++03 -fpermissive" ./configure
```

然后再编译:

```
make V=1
```

#### 编译结果

```
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

#### 原因分析

C++ 过新导致语法不兼容，例如:

```
- Iter tmp = current;
+ Iter tmp = this->current;
```
