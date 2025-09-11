# Install abinit on RevyOS

## Fetch source code
```bash
wget https://forge.abinit.org/abinit-10.4.7.tar.gz
tar xf abinit-10.4.7.tar.gz
```

## Building
```bash
./configure
make
make install
```

## Results

`configure` yields the following error:

```log
checking for Fortran libraries of mpifort...  -L/usr/lib/riscv64-linux-gnu/openmpi/lib -L/usr/lib/gcc/riscv64-linux-gnu/14 -L/lib/riscv64-linux-gnu -L/usr/lib/riscv64-linux-gnu -lmpi_usempif08 -lmpi_usempi_ignore_tkr -lmpi_mpifh -lmpi -lgfortran -lm
checking for dummy main to link with Fortran libraries... unknown
configure: error: in `/home/debian/abinit-10.4.7':
configure: error: linking to Fortran libraries from C fails
See `config.log' for more details

```

`config.log` shows multiple errors in the following form:

```log
config.log:975:configure:20602: mpicc -o conftest -g -O2 -march=native        conftest.c   -L/usr/lib/riscv64-linux-gnu/openmpi/lib -L/usr/lib/gcc/riscv64-linux-gnu/14 -L/lib/riscv64-linux-gnu -L/usr/lib/riscv64-linux-gnu -lmpi_usempif08 -lmpi_usempi_ignore_tkr -lmpi_mpifh -lmpi -lgfortran -lm >&5
config.log:976:gcc: error: '-march=native': ISA string must begin with rv32, rv64 or Profiles
```

This is due to incorrect `march` argument handling with the current RISC-V toolchain. Fixing this may require editing `configure.ac` and regenerating `configure` from `autoconf`, which is not possible at the moment after a few attempts.