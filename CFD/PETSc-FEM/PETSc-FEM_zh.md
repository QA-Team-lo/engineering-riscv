# PETSC-FEM (CFD) on RevyOS

## 安装准备

```
sudo apt install petsc-dev lapack metis libmeschach1.2 ann-tools libann0
wget https://cimec.org.ar/~mstorti/petscfem-snapshots/petscfem-last.tgz
tar -xvzf petscfem-last.tgz
```

## 编译和安装

为适配 3.22 版本 petsc,修改`Makefile.defs`文件为 `PETSC_DIR := /usr/share/petsc/3.22/lib/petsc` 
发现编译失败,猜测为 petsc 在 riscv 平台上编译存在问题, 缺少部分所需文件

```
make
Makefile.base:97: /usr/share/petsc/3.22/lib/petsc/conf/variables: No such file or directory
make: *** No rule to make target '/usr/share/petsc/3.22/lib/petsc/conf/variables'.  Stop.
```
