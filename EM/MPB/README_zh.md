# 指南：在 RevyOS 上安装 MIT Photonic Bands (MPB)

本文档提供了在运行 RevyOS 的 RISC-V 设备上安装 MIT Photonic Bands (MPB) 软件的说明。

## 第一步: 安装

幸运的是, MPB 已经存在于 RevyOS 仓库中:

```log
mpb/trixie 1.11.1-6+b2 riscv64
  MIT Photonic-Bands

mpb-dbgsym/trixie 1.11.1-6+b2 riscv64
  debug symbols for mpb

mpb-dev/trixie 1.11.1-6+b2 riscv64
  MIT Photonic-Bands development files

mpb-doc/trixie 1.11.1-6 all
  MIT Photonic-Bands documentation

mpb-mpi/trixie 1.11.1-6+b2 riscv64
  MIT Photonic-Bands, parallel (mpich) version

mpb-mpi-dbgsym/trixie 1.11.1-6+b2 riscv64
  debug symbols for mpb-mpi

mpb-scm/trixie 1.11.1-6 all
  MIT Photonic-Bands initialisation files
```

使用以下命令安装：

```bash
apt install mpb
```

## 结果

```bash
debian@revyos-pioneer:~$ mpb -V
mpb 1.11.1, Copyright (C) 1999-2012, MIT
Using libctl 4.5.1 and Guile 3.0.10.
debian@revyos-pioneer:~$ mpb
GNU Guile 3.0.10
Copyright (C) 1995-2024 Free Software Foundation, Inc.

Guile comes with ABSOLUTELY NO WARRANTY; for details type `,show w'.
This program is free software, and you are welcome to redistribute it
under certain conditions; type `,show c' for details.

Enter `,help' for help.
mpb>

```