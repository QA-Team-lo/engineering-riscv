# Install LAMMPS on RevyOS

Luckily, LAMMPS is already present in the RevyOS repository:

```log
lammps/trixie,now 20250204+dfsg.1-2 riscv64 [已安装]
  Molecular Dynamics Simulator

lammps-data/trixie,now 20250204+dfsg.1-2 all [已安装，自动]
  Molecular Dynamics Simulator. Data (potentials)
```

Install it with:

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

Consult [docs.lammps.org/Run_basics.html](docs.lammps.org/Run_basics.html) for more usage information.