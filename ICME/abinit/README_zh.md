# 指南：在 RevyOS 上安装 abinit

本文档提供了在运行 RevyOS 的 RISC-V 设备上构建 abinit 软件的说明。

## 第一步: 获取源代码
```bash
wget https://forge.abinit.org/abinit-10.4.7.tar.gz
tar xf abinit-10.4.7.tar.gz
```

## 第二步: 构建
```bash
./configure
make
make install
```

## 结果

`configure` 产生以下错误:

```log
checking for Fortran libraries of mpifort...  -L/usr/lib/riscv64-linux-gnu/openmpi/lib -L/usr/lib/gcc/riscv64-linux-gnu/14 -L/lib/riscv64-linux-gnu -L/usr/lib/riscv64-linux-gnu -lmpi_usempif08 -lmpi_usempi_ignore_tkr -lmpi_mpifh -lmpi -lgfortran -lm
checking for dummy main to link with Fortran libraries... unknown
configure: error: in `/home/debian/abinit-10.4.7':
configure: error: linking to Fortran libraries from C fails
See `config.log' for more details

```

`config.log` 以以下形式显示多个错误:

```log
config.log:975:configure:20602: mpicc -o conftest -g -O2 -march=native        conftest.c   -L/usr/lib/riscv64-linux-gnu/openmpi/lib -L/usr/lib/gcc/riscv64-linux-gnu/14 -L/lib/riscv64-linux-gnu -L/usr/lib/riscv64-linux-gnu -lmpi_usempif08 -lmpi_usempi_ignore_tkr -lmpi_mpifh -lmpi -lgfortran -lm >&5
config.log:976:gcc: error: '-march=native': ISA string must begin with rv32, rv64 or Profiles
```

这是由于当前 RISC-V 工具链对 `march` 参数的处理不正确造成的. 修复此问题可能需要编辑 `configure.ac` 并从 `autoconf` 重新生成 `configure`, 目前尝试了几次后仍无法完成.