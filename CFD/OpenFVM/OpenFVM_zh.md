# OpenFVM (CFD) on RevyOS

## 安装准备

```
sudo apt install lammps gmsh metis
```

## 获取源代码
```
curl -L "https://sourceforge.net/projects/openfvm/files/OpenFVM%20Source/v1.1/OpenFVM-v1.1.tgz/download" -o OpenFVM-v1.1.tgz
tar -xvzf OpenFVM-v1.1.tgz
```

## 编译和安装

为了执行编译,`Flow/parallel`目录中的 makefile 需要更新其库路径以进行正确的链接.
但由于基础库文件结构发生了更改，使当前的 makefile 无法使用进行编译。需要完全重写 makefile

```
debian@revyos-pioneer ~/C/OpenFVM> cd Flow/parallel/
debian@revyos-pioneer ~/C/O/F/parallel> make
makefile:64: /bmake/common/base: No such file or directory
make: *** No rule to make target '/bmake/common/base'.  Stop.
```
