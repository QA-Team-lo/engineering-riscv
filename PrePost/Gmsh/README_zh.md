### **指南：在 RevyOS 上安装 GMSH**

本文档提供了在运行 RevyOS Debian 衍生物的 RISC-V 设备上安装 `GMSH` 有限元分析 (FEA) 前处理和后处理工具的说明。由于 `GMSH` 包在 RevyOS 仓库中可用，安装过程将很简单。

```bash
debian@revyos-lpi4a:~$ sudo apt install gmsh
Installing:                     
  gmsh

Installing dependencies:
  gmsh-doc       libfltk-images1.3  libgl2ps1.4                libocct-draw-7.6                 libocct-modeling-data-7.6  libtcl8.6   occt-misc
  libalglib3.19  libfltk1.3         libgmsh4.8                 libocct-foundation-7.6           libocct-ocaf-7.6           libtk8.6
  libfltk-gl1.3  libfreeimage3      libocct-data-exchange-7.6  libocct-modeling-algorithms-7.6  libocct-visualization-7.6  libvoro++1

Suggested packages:
  tcl8.6  tk8.6

Summary:
  Upgrading: 0, Installing: 20, Removing: 0, Not Upgrading: 0
  Download size: 44.5 MB
  Space needed: 171 MB / 94.8 GB available

Continue? [Y/n] 
```

![GMSH 工作界面](images/gmsh-working.png)

`GMSH` 包现已安装并准备使用，渲染和工作正常但有点慢。
