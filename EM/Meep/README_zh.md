# 指南：在 RevyOS 上安装 Meep

本文档提供了在运行 RevyOS 的 RISC-V 设备上安装 Meep 软件的说明。

## 第一步: 安装

幸运的是, Meep 已经存在于 RevyOS 仓库中，以及其各种库版本和 Python 实现:

```log
libmeep-dev/trixie 1.29.0-2 riscv64
  development library for using meep

libmeep-mpi-default-dev/trixie 1.29.0-2 riscv64
  development library for using parallel version of meep

libmeep-mpi-default33/trixie 1.29.0-2 riscv64
  library for using parallel version of meep

libmeep-mpi-default33-dbgsym/trixie 1.29.0-2 riscv64
  debug symbols for libmeep-mpi-default33

libmeep33/trixie,now 1.29.0-2 riscv64 [已安装，自动]
  library for using meep

libmeep33-dbgsym/trixie 1.29.0-2 riscv64
  debug symbols for libmeep33

meep/trixie,now 1.29.0-2 riscv64 [已安装]
  software package for FDTD simulation

meep-dbgsym/trixie 1.29.0-2 riscv64
  debug symbols for meep

meep-mpi-default/trixie 1.29.0-2 riscv64
  software package for FDTD simulation, parallel version

meep-mpi-default-dbgsym/trixie 1.29.0-2 riscv64
  debug symbols for meep-mpi-default

python3-meep/trixie 1.29.0-2 riscv64
  software package for FDTD simulation with Python

python3-meep-dbgsym/trixie 1.29.0-2 riscv64
  debug symbols for python3-meep

python3-meep-mpi-default/trixie 1.29.0-2 riscv64
  software package for FDTD simulation with Python

```

使用以下命令安装：

```bash
apt install meep
```

## 结果

```bash
debian@revyos-pioneer:~$ meep --version
Meep 1.29.0, Copyright (C) 2005-2022 Massachusetts Institute of Technology.
Using libctl 4.5.1 and Guile 3.0.10.

Elapsed run time = 0.559366 s
debian@revyos-pioneer:~$ meep
GNU Guile 3.0.10
Copyright (C) 1995-2024 Free Software Foundation, Inc.

Guile comes with ABSOLUTELY NO WARRANTY; for details type `,show w'.
This program is free software, and you are welcome to redistribute it
under certain conditions; type `,show c' for details.

Enter `,help' for help.
meep>

```