# RISC-V RevyOS/Debian 工程软件

在 RISC-V RevyOS/Debian 上构建、安装和测试工程软件。

## 偏微分方程 (PDE) 求解器

| 类别                 | 目录                              |
|----------------------|-----------------------------------|
| 通用有限元分析 (FEA) | [FEA](./FEA/README_zh.md)         |
| 计算流体力学 (CFD)   | [CFD](./CFD/README_zh.md)         |
| 电磁学和光学         | [EM](./EM/README_zh.md)           |
| 相场模拟软件         | [SPFSims](./SPFSims/README_zh.md) |
| 边界元方法 (BEM)     | [BEM](./BEM/README_zh.md)         |
| 前处理和后处理框架   | [PrePost](./PrePost/README_zh.md) |

### 通用有限元分析 (FEA)

| 名称                                                                                                                         | 描述                                                                                    | 作者                  | 许可证   | 可用性状态 |
|------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|-----------------------|----------|------------|
| [Elmer](https://www.csc.fi/web/elmer) ([profile](https://www.opennovation.org/profiles/Elmer.html))                          | 多物理场问题的 FEA 软件                                                                 |                       | GPL      | 成功       |
| [Code-Aster](https://www.code-aster.org/V2/spip.php?rubrique2) ([profile](https://www.opennovation.org/profiles/aster.html)) | 结构和热机械软件                                                                        | Electricite de France | GPL      | 失败       |
| [CalculiX](http://www.calculix.de/)                                                                                          | 三维结构有限元程序                                                                      | Several               | GPL      | 成功       |
| [FreeFEM](https://www.freefem.org/)                                                                                          | 有限元软件系列，包括 [FFW](https://www.freefem.org/ff++/ff++/web/) (FreeFEM on the Web) |                       | GPL      | 成功       |
| [Impact](http://impact.sourceforge.net/)                                                                                     | 显式动态有限元程序                                                                      | Several               | GPL      | 成功       |
| [deal.II](https://www.dealii.org/) ([profile](https://www.opennovation.org/profiles/deal.II.html))                           | 使用自适应 FEA 求解 PDE 的 C++ 库                                                       |                       |          | 失败       |
| [XmdS](https://www.xmds.org/)                                                                                                | 可扩展的多维模拟器                                                                      |                       | GPL      | 成功       |
| [GetDP](http://www.geuz.org/getdp/)                                                                                          | 处理离散问题的通用环境                                                                  |                       | GPL      | 成功       |
| [FElt](http://felt.sourceforge.net/)                                                                                         | 固体力学 FEA                                                                            |                       | GPL      | 失败       |
| [OOFEM](http://www.oofem.org/)                                                                                               | 通用有限元程序                                                                          |                       | GPL      | 成功       |
| [TOCHNOG](http://tochnog.sourceforge.net/)                                                                                   | 免费有限元程序                                                                          | Dennis Roddeman       | GPL      | 成功       |
| [FEniCS](https://fenicsproject.org/)                                                                                         | 自动化 ODE/PDE 求解器                                                                   | FEniCS group          | GPL+LGPL | 成功       |
| [SLFFEA](http://slffea.sourceforge.net/)                                                                                     | 结构有限元分析求解器                                                                    |                       | GPL      | 成功       |
| [FELyX](http://felyx.sourceforge.net/)                                                                                       | 通用有限元方法工具箱                                                                    |                       | GPL      | 失败       |
| [ALBERTA](http://www.alberta-fem.de/)                                                                                        | 通用有限元方法库                                                                        |                       | GPL3     | 成功       |

### 计算流体力学 (CFD)

| 名称                                                                                                       | 描述                                                                       | 作者                                                             | 许可证   | 可用性状态 |
|------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|------------------------------------------------------------------|----------|------------|
| [OpenFOAM](https://openfoam.com/)                                                                          | 带有预处理器的通用 CFD 工具箱                                              | [OpenFOAM](https://openfoam.com/)                                | GPL      | 成功       |
| [OpenFlower](http://openflower.sourceforge.net/)                                                           | 专注于湍流非定常不可压缩 Navier-Stokes 方程的 CFD 求解器                   |                                                                  | GPL      | 失败       |
| [Gerris](http://gfs.sourceforge.net/)                                                                      | 具有自适应网格细化的变密度不可压缩 Navier-Stokes、Stokes 或 Euler 求解器   | New Zealand National Institute of Water and Atmospheric Research | GPL      | 失败       |
| [Code_Saturne](https://www.code-saturne.org/)                                                              | 通用 CFD 软件                                                              | Electricite de France                                            | GPL      | 成功       |
| [libMesh](http://libmesh.sourceforge.net/) ([profile](https://www.opennovation.org/profiles/libMesh.html)) | 基于 [PETSc](https://petsc.org/release/) 的具有自适应网格细化的 C++ FEA 库 |                                                                  |          | 失败       |
| [DUNS](http://duns.sourceforge.net/)                                                                       | 对角化迎风 Navier Stokes 代码                                              | Pennsylvania State University                                    | GPL      | 失败       |
| [SLFCFD](http://slfcfd.sourceforge.net/)                                                                   | San Le's Free Computational Fluid Dynamics                                 |                                                                  | GPL      | 失败       |
| [PETSc-FEM](https://www.cimec.org.ar/twiki/bin/view/Cimec/PETScFEM)                                        | 基于 [PETSc](https://petsc.org/release/) 的通用多物理场 FEM 包             |                                                                  | GPL      | 失败       |
| [TYPHON](http://typhon.sourceforge.net/)                                                                   | 用于气体动力学多种计算方法的开发平台                                       |                                                                  | GPL      | 失败       |
| [OpenFVM](http://openfvm.sourceforge.net/)                                                                 |                                                                            |                                                                  | GPL      | 失败       |
| [ADFC](http://adfc.sourceforge.net/index_en.html)                                                          |                                                                            |                                                                  | GPL      | 失败       |
| [Dolfyn](http://www.dolfyn.net/dolfyn/index_en.html)                                                       |                                                                            |                                                                  | Apache 2 | 成功           |

### 电磁学和光学

| 名称                                                                              | 描述                                       | 作者                                                      | 许可证 | 可用性状态 |
|-----------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------------------------------|--------|------------|
| Tessa                                                                             | 基于 FDTD 方法的光学系统三维模拟软件       |                                                           |        | 失败       |
| [Meep](https://meep.readthedocs.io/en/latest/)                                    | 用于电磁系统的有限差分时域 (FDTD) 模拟软件 | Joannopoulos Ab Initio Physics group                      | GPL    | 成功       |
| [MIT Photonic Bands](https://ab-initio.mit.edu/wiki/index.php/MIT_Photonic_Bands) | 计算周期性介电结构的能带结构和电磁模式     | Steven G. Johnson 和 Joannopoulos Ab Initio Physics group | GPL    | 成功       |

### 相场模拟软件

| 名称      | 描述                       | 作者           | 许可证        | 可用性状态 |
|-----------|----------------------------|----------------|---------------|------------|
| FiPy      | Python 有限体积 PDE 求解器 | NIST CTCMS     | Public domain | 成功       |
| RheoPlast | 并行 C PDE 求解器          | Adam Powell 等 | GPL           | 失败       |

### 边界元方法 (BEM)

| 名称   | 描述                                       | 作者                         | 许可证 | 可用性状态 |
|--------|--------------------------------------------|------------------------------|--------|------------|
| Julian | 用于拉普拉斯方程和线性弹性力学的边界元代码 | Adam Powell 和 Yi-Cheung Lok | GPL    | 失败       |

### 前处理和后处理框架

| 名称                                                                                                      | 描述                                           | 作者                                           | 许可证  | 可用性状态 |
|-----------------------------------------------------------------------------------------------------------|------------------------------------------------|------------------------------------------------|---------|------------|
| [Salome](https://www.salome-platform.org/) ([profile](https://www.opennovation.org/profiles/Salome.html)) | 具有一些 CAD 功能的 FEA 前处理和后处理图形框架 | Several                                        | LGPL    | 失败       |
| [Gmsh](http://www.geuz.org/gmsh/) ([profile](https://www.opennovation.org/profiles/Gmsh.html))            | 图形 FEA CAD 工具、网格生成器、后处理器        | Christophe Geuzaine 和 Jean-Francois Remacle   | GPL     | 成功       |
| OpenCASCADE ([profile](https://www.opennovation.org/profiles/OpenCASCADE.html))                           | 高级 CAD 库                                    | Open CASCADE S.A.S.                            | OCTPL   | 成功       |
| NETGEN                                                                                                    | 自动 2D 或 3D 网格生成器                       | Joachim Shoberl                                | LGPL    | 成功       |
| [enGrid](http://engrid.sourceforge.net/)                                                                  | 自动 2D 或 3D 网格生成器                       |                                                | GPL     | 失败       |
| [MeshLab](http://meshlab.sourceforge.net/)                                                                | 处理和编辑非结构化 3D 三角网格的系统           | Paolo Cignoni 等                               | GPL     | 成功       |
| [Paraview](https://www.paraview.org/)                                                                     | 并行可视化应用程序                             | Kitware 等                                     | several | 失败       |
| [Caret](http://brainvis.wustl.edu/wiki/index.php/Caret:About)                                             | 可视化工具                                     | Visualization group, Washington University     | GPL     | 失败       |
| [FSLView](https://fsl.fmrib.ox.ac.uk/fsl/fslview/)                                                        | 面向医学 MRI 的体积数据可视化工具              | Analysis Group, FMRIB, Oxford, UK              | GPL     | 成功       |
| Illuminator                                                                                               | 结构化网格数据集的并行可视化库                 | Adam Powell 等                                 | LGPL    | 失败       |
| [MayaVi](http://mayavi.sourceforge.net/) 和 [Mayavi2](http://code.enthought.com/projects/mayavi/)         | 基于 VTK 的数据可视化工具                      | Enthought                                      | BSD     | 成功       |
| [VisIt](https://visit-dav.github.io/visit-website/)                                                       | 并行可视化工具                                 | WCI (Lawrence Livermore National Laboratories) | BSD     | 失败       |
| [VisTrails](https://www.vistrails.org/index.php/Main_Page)                                                |                                                |                                                | GPL     | 失败       |
| [Discretizer](http://www.discretizer.org/)                                                                |                                                |                                                | GPLv3   | 失败       |

## 计算机辅助设计 (CAD)

| 类别                 | 目录                      |
|----------------------|---------------------------|
| 计算机辅助设计 (CAD) | [CAD](./CAD/README_zh.md) |
| 多体动力学           | [MBD](./MBD/README_zh.md) |

### 计算机辅助设计 (CAD)

| 名称                                                     | 描述                                          | 作者                                                 | 许可证 | 可用性状态 |
|----------------------------------------------------------|-----------------------------------------------|------------------------------------------------------|--------|------------|
| [BRL-CAD](https://brlcad.org/)                           | 美国军方使用的成熟构造实体几何 (CSG) CAD 系统 | U.S. Army Basic Research Laboratories                | GPL    | 失败       |
| [VARKON](http://varkon.sourceforge.net/)                 | 高级 CAD 系统                                 | Orebro university Department of Technology CAD group | LGPL   | 失败       |
| [QCad](https://qcad.org/en/qcad)                         | 使用 Qt 小部件工具包的 2D 通用 CAD 系统       | RibbonSoft, GmbH                                     | GPL    | 成功       |
| [Sweet Home 3D](http://sweethome3d.sourceforge.net/)     | 室内设计 CAD 软件                             | eTeks                                                | GPL    | 成功       |
| [PythonCAD](https://sourceforge.net/projects/pythoncad/) | 用 Python 编写的 2D 通用 CAD 系统             | matteoboscolo                                        | GPL    | 失败       |
| [SagCAD](http://sagcad.sourceforge.net/)                 | 简单易用的 2D CAD/CAM 软件                    | Sagiya Metal Mold Factory, Inc.                      | GPL    | 成功       |
| [Sailcut CAD](http://www.sailcut.com/Sailcut_CAD)        | 用于设计和可视化帆的软件                      | Robert Lainé and Jeremy Lainé                        | GPL    | 成功       |

## 数据分析

| 类别     | 目录                                        |
|----------|---------------------------------------------|
| 数据分析 | [DataAnalysis](./DataAnalysis/README_zh.md) |

### 数据分析

| 名称                                 | 描述               | 作者                 | 许可证 | 可用性状态 |
|--------------------------------------|--------------------|----------------------|--------|------------|
| [Gpiv](http://gpiv.sourceforge.net/) | 粒子图像测速 (PIV) | Gerber van der Graaf | GPL    | CFT        |

## 管道和排水清洁

| 类别           | 目录                                |
|----------------|-------------------------------------|
| 管道和排水清洁 | [Plumbing](./Plumbing/README_zh.md) |

## 集成计算材料工程 (ICME)

| 类别                    | 目录                        |
|-------------------------|-----------------------------|
| 集成计算材料工程 (ICME) | [ICME](./ICME/README_zh.md) |

### 集成计算材料工程 (ICME)

| 名称                              | 描述                                             | 作者              | 许可证 | 可用性状态 |
|-----------------------------------|--------------------------------------------------|-------------------|--------|------------|
| [abinit](https://www.abinit.org/) | 用于分子和晶体的密度泛函理论 (DFT)，包括几何优化 | Xavier Gonze, UCL | GPL    | 失败       |
| [LAMMPS](https://www.lammps.org/) | 并行分子动力学代码                               |                   | GPL    | 成功       |
