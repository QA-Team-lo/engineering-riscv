# PETSC-FEM (CFD) on RevyOS

## Prerequisites

```
sudo apt install petsc-dev lapack metis libmeschach1.2 ann-tools libann0
wget https://cimec.org.ar/~mstorti/petscfem-snapshots/petscfem-last.tgz
tar -xvzf petscfem-last.tgz
```

## Compile and Install

The Makefile.defs file was updated to support PETSc version 3.22 by setting `PETSC_DIR := /usr/share/petsc/3.22/lib/petsc`. 
Subsequent compilation attempts were unsuccessful, suggesting potential issues with PETSc's build process on the RISC-V platform, possibly stemming from missing files.


```
make
Makefile.base:97: /usr/share/petsc/3.22/lib/petsc/conf/variables: No such file or directory
make: *** No rule to make target '/usr/share/petsc/3.22/lib/petsc/conf/variables'.  Stop.
```
