# OpenFVM (CFD) on RevyOS

## Prerequisites

```
sudo apt install lammps gmsh metis
```

## Obtain the Source Code
```
curl -L "https://sourceforge.net/projects/openfvm/files/OpenFVM%20Source/v1.1/OpenFVM-v1.1.tgz/download" -o OpenFVM-v1.1.tgz
tar -xvzf OpenFVM-v1.1.tgz
```

## Compile and Install

To proceed, the makefile in the `Flow/parallel` directory needs to have its library paths updated for correct linking. 
Unfortunately, the underlying library file structure has undergone changes, rendering the current makefile unusable for compilation. As a result, a complete rewrite of the makefile is required.

```
debian@revyos-pioneer ~/C/OpenFVM> cd Flow/parallel/
debian@revyos-pioneer ~/C/O/F/parallel> make
makefile:64: /bmake/common/base: No such file or directory
make: *** No rule to make target '/bmake/common/base'.  Stop.
```
