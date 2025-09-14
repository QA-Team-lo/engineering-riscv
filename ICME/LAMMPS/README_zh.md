# 指南：在 RevyOS 上安装 LAAMMPS

本文档提供了在运行 RevyOS 的 RISC-V 设备上安装 LAMMPS 软件的说明。

## 第一步: 安装

幸运的是, LAMMPS 已经存在于 RevyOS 仓库中:

```log
lammps/trixie,now 20250204+dfsg.1-2 riscv64 [已安装]
  Molecular Dynamics Simulator

lammps-data/trixie,now 20250204+dfsg.1-2 all [已安装，自动]
  Molecular Dynamics Simulator. Data (potentials)
```

使用以下命令安装：

```bash
apt install lammps
```

## Results

```bash
debian@revyos-pioneer:~$ lmp
LAMMPS (4 Feb 2025)
OMP_NUM_THREADS environment is not set. Defaulting to 1 thread. (src/comm.cpp:99)
  using 1 OpenMP thread(s) per MPI task
```

有关更多使用信息, 请参阅[docs.lammps.org/Run_basics.html](docs.lammps.org/Run_basics.html).