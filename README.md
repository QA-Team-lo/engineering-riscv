# Engineering Software for RISC-V RevyOS/Debian

Build, install and test engineering software on RISC-V RevyOS/Debian.

## Partial Differential Equation (PDE) Solvers

| Category                              | Directory                      |
|---------------------------------------|--------------------------------|
| General Finite Element Analysis (FEA) | [FEA](./FEA/README.md)         |
| Computational Fluid Dynamics (CFD)    | [CFD](./CFD/README.md)         |
| Electromagnetism and Optics           | [EM](./EM/README.md)           |
| Software for Phase Field simulations  | [SPFSims](./SPFSims/README.md) |
| Boundary Element Method (BEM)         | [BEM](./BEM/README.md)         |
| Pre- and post-processing frameworks   | [PrePost](./PrePost/README.md) |

### General Finite Element Analysis (FEA)

| Name                                                                                                                         | Description                                                                                                  | Author                | License  | Status                             |
|------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|-----------------------|----------|------------------------------------|
| [Elmer](https://www.csc.fi/web/elmer) ([profile](https://www.opennovation.org/profiles/Elmer.html))                          | FEA Software for Multiphysics Problems                                                                       |                       | GPL      | [SUCC](./FEA/Elmer/README.md)      |
| [Code-Aster](https://www.code-aster.org/V2/spip.php?rubrique2) ([profile](https://www.opennovation.org/profiles/aster.html)) | Structural and thermomechanical software                                                                     | Electricite de France | GPL      | [FAIL](./FEA/Code-Aster/README.md) |
| [CalculiX](http://www.calculix.de/)                                                                                          | Three-Dimensional Structural Finite Element Program                                                          | Several               | GPL      | [SUCC](./FEA/CalculiX/README.md)   |
| [FreeFEM](https://www.freefem.org/)                                                                                          | Finite element software family, including [FFW](https://www.freefem.org/ff++/ff++/web/) (FreeFEM on the Web) |                       | GPL      | [SUCC](./FEA/FreeFEM/README.md)    |
| [Impact](http://impact.sourceforge.net/)                                                                                     | Explicit dynamic finite element program                                                                      | Several               | GPL      | [SUCC](./FEA/Impact/README.md)     |
| [deal.II](https://www.dealii.org/) ([profile](https://www.opennovation.org/profiles/deal.II.html))                           | C++ library for solving PDEs using adaptive FEA                                                              |                       |          | [FAIL](./FEA/deal-II/README.md)    |
| [XmdS](https://www.xmds.org/)                                                                                                | Extensible multi-dimensional simulator                                                                       |                       | GPL      | [SUCC](./FEA/XmdS/README.md)       |
| [GetDP](http://www.geuz.org/getdp/)                                                                                          | Generalized environment for treatment of discrete problems                                                   |                       | GPL      | [SUCC](./FEA/GetDP/README.md)      |
| [FElt](http://felt.sourceforge.net/)                                                                                         | Solid mechanics FEA                                                                                          |                       | GPL      | [FAIL](./FEA/FElt/README.md)       |
| [OOFEM](http://www.oofem.org/)                                                                                               | General finite element program                                                                               |                       | GPL      | [SUCC](./FEA/OOFEM/README.md)      |
| [TOCHNOG](http://tochnog.sourceforge.net/)                                                                                   | Free finite element program                                                                                  | Dennis Roddeman       | GPL      | [SUCC](./FEA/TOCHNOG/README.md)    |
| [FEniCS](https://fenicsproject.org/)                                                                                         | Automated ODE/PDE solver                                                                                     | FEniCS group          | GPL+LGPL | [SUCC](./FEA/FEniCS/README.md)     |
| [SLFFEA](http://slffea.sourceforge.net/)                                                                                     | Structural finite element analysis solver                                                                    |                       | GPL      | [SUCC](./FEA/SLFFEA/README.md)     |
| [FELyX](http://felyx.sourceforge.net/)                                                                                       | General finite element method toolbox                                                                        |                       | GPL      | [FAIL](./FEA/FELyX/README.md)      |
| [ALBERTA](http://www.alberta-fem.de/)                                                                                        | General finite element method library                                                                        |                       | GPL3     | [SUCC](./FEA/ALBERTA/README.md)    |

### Computational Fluid Dynamics (CFD)

| Name                                                                                                       | Description                                                                                         | Author                                                           | License  | Status                                     |
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|------------------------------------------------------------------|----------|--------------------------------------------|
| [OpenFOAM](https://openfoam.com/)                                                                          | General CFD toolbox with pre-processor                                                              | [OpenFOAM](https://openfoam.com/)                                | GPL      | [SUCC](./CFD/OpenFOAM/OpenFOAM.md)         |
| [OpenFlower](http://openflower.sourceforge.net/)                                                           | CFD solver focused on turbulent unsteady incompressible Navier-Stokes equations                     |                                                                  | GPL      | [FAIL](./CFD/OpenFlower/OpenFlower.md)     |
| [Gerris](http://gfs.sourceforge.net/)                                                                      | Variable density incompressible Navier-Stokes, Stokes or Euler solver with adaptive mesh refinement | New Zealand National Institute of Water and Atmospheric Research | GPL      | [FAIL](./CFD/Gerris/Gerris.md)             |
| [Code\_Saturne](https://www.code-saturne.org/)                                                             | General purpose CFD software                                                                        | Electricite de France                                            | GPL      | [SUCC](./CFD/code_saturne/code_saturne.md) |
| [libMesh](http://libmesh.sourceforge.net/) ([profile](https://www.opennovation.org/profiles/libMesh.html)) | C++ FEA library with adaptive mesh refinement based on [PETSc](https://petsc.org/release/)          |                                                                  |          | [FAIL](./CFD/libMesh/libMesh.md)           |
| [DUNS](http://duns.sourceforge.net/)                                                                       | Diagonalized Upwind Navier Stokes Code                                                              | Pennsylvania State University                                    | GPL      | [FAIL](./CFD/DUNS/DUNS.md)                 |
| [SLFCFD](http://slfcfd.sourceforge.net/)                                                                   | San Le's Free Computational Fluid Dynamics                                                          |                                                                  | GPL      | [FAIL](./CFD/SLFCFD/SLFCFD.md)             |
| [PETSc-FEM](https://www.cimec.org.ar/twiki/bin/view/Cimec/PETScFEM)                                        | General multi-physics FEM package based on [PETSc](https://petsc.org/release/)                      |                                                                  | GPL      | [FAIL](./CFD/PETSc-FEM/PETSc-FEM.md)       |
| [TYPHON](http://typhon.sourceforge.net/)                                                                   | Development platform for many computational methods for gas dynamics                                |                                                                  | GPL      | [FAIL](./CFD/TYPHON/TYPHON.md)             |
| [OpenFVM](http://openfvm.sourceforge.net/)                                                                 |                                                                                                     |                                                                  | GPL      | [FAIL](./CFD/OpenFVM/OpenFVM.md)           |
| [ADFC](http://adfc.sourceforge.net/index_en.html)                                                          |                                                                                                     |                                                                  | GPL      | [FAIL](./CFD/ADFC/ADFC.md)                 |
| [Dolfyn](http://www.dolfyn.net/dolfyn/index_en.html)                                                       |                                                                                                     |                                                                  | Apache 2 | [SUCC](./CFD/Dolfyn/Dolfyn.md)             |

###  Electromagnetism and Optics

| Name                                                                              | Description                                                                              | Author                                                         | License | Status                       |
|-----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------|---------|------------------------------|
| Tessa                                                                             | Three-dimensional simulation software for optical systems, based on the FDTD method      |                                                                |         | [FAIL](./EM/Tessa/README.md) |
| [Meep](https://meep.readthedocs.io/en/latest/)                                    | Finite-difference time-domain (FDTD) simulation software for electromagnetic systems     | Joannopoulos Ab Initio Physics group                           | GPL     | [SUCC](./EM/Meep/README.md)  |
| [MIT Photonic Bands](https://ab-initio.mit.edu/wiki/index.php/MIT_Photonic_Bands) | Computes the band structures and electromagnetic modes of periodic dielectric structures | Steven G. Johnson and the Joannopoulos Ab Initio Physics group | GPL     | [SUCC](./EM/MPB/README.md)   |

### Software for Phase Field simulations

| Name      | Description                     | Author             | License       | Status |
|-----------|---------------------------------|--------------------|---------------|--------|
| FiPy      | Python finite volume PDE solver | NIST CTCMS         | Public domain | [SUCC](./SPFSims/FiPy/README.md)   |
| RheoPlast | Parallel C PDE solver           | Adam Powell et al. | GPL           | [FAIL](./SPFSims/RheoPlast/README.md)   |

### Boundary Element Method (BEM)

| Name   | Description                                                             | Author                        | License | Status |
|--------|-------------------------------------------------------------------------|-------------------------------|---------|--------|
| Julian | Boundary element code for Laplace equation and linear elastic mechanics | Adam Powell and Yi-Cheung Lok | GPL     | [FAIL](./BEM/Julian/README.md)   |

### Pre- and post-processing frameworks

| Name                                                                                                      | Description                                                                     | Author                                         | License | Status                                  |
|-----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|------------------------------------------------|---------|-----------------------------------------|
| [Salome](https://www.salome-platform.org/) ([profile](https://www.opennovation.org/profiles/Salome.html)) | Graphical framework for FEA pre- and post-processing with some CAD capabilities | Several                                        | LGPL    | [FAIL](./PrePost/Salome/README.md)      |
| [Gmsh](http://www.geuz.org/gmsh/) ([profile](https://www.opennovation.org/profiles/Gmsh.html))            | Graphical FEA CAD tool, mesher, post-processor                                  | Christophe Geuzaine and Jean-Francois Remacle  | GPL     | [SUCC](./PrePost/Gmsh/README.md)        |
| OpenCASCADE ([profile](https://www.opennovation.org/profiles/OpenCASCADE.html))                           | High-level CAD library                                                          | Open CASCADE S.A.S.                            | OCTPL   | [SUCC](./PrePost/OpenCASCADE/README.md) |
| NETGEN                                                                                                    | Automatic 2-D or 3-D mesh generator                                             | Joachim Shoberl                                | LGPL    | [SUCC](./PrePost/NETGEN/README.md)      |
| [enGrid](http://engrid.sourceforge.net/)                                                                  | Automatic 2-D or 3-D mesh generator                                             |                                                | GPL     | [FAIL](./PrePost/enGrid/README.md)      |
| [MeshLab](http://meshlab.sourceforge.net/)                                                                | System for processing and editing unstructured 3D triangular meshes             | Paolo Cignoni et al.                           | GPL     | [SUCC](./PrePost/MeshLab/README.md)     |
| [Paraview](https://www.paraview.org/)                                                                     | Parallel visualization application                                              | Kitware et al.                                 | several | [FAIL](./PrePost/Paraview/README.md)    |
| [Caret](http://brainvis.wustl.edu/wiki/index.php/Caret:About)                                             | Visualization tool                                                              | Visualization group, Washington University     | GPL     | [FAIL](./PrePost/Caret/README.md)       |
| [FSLView](https://fsl.fmrib.ox.ac.uk/fsl/fslview/)                                                        | Visualization tool for volume data geared toward medical MRI                    | Analysis Group, FMRIB, Oxford, UK              | GPL     | [SUCC](./PrePost/FSLView/README.md)     |
| Illuminator                                                                                               | Parallel visualization library for structured grid data sets                    | Adam Powell et al.                             | LGPL    | [FAIL](./PrePost/Illuminator/README.md) |
| [MayaVi](http://mayavi.sourceforge.net/) and [Mayavi2](http://code.enthought.com/projects/mayavi/)        | Data visualization tools based on VTK                                           | Enthought                                      | BSD     | [SUCC](./PrePost/MayaVi/README.md)      |
| [VisIt](https://visit-dav.github.io/visit-website/)                                                       | Parallel visualization tool                                                     | WCI (Lawrence Livermore National Laboratories) | BSD     | [FAIL](./PrePost/VisIt/README.md)       |
| [VisTrails](https://www.vistrails.org/index.php/Main_Page)                                                |                                                                                 |                                                | GPL     | [FAIL](./PrePost/VisTrails/README.md)   |
| [Discretizer](http://www.discretizer.org/)                                                                |                                                                                 |                                                | GPLv3   | [FAIL](./PrePost/Discretizer/README.md) |

## Computer-Aided Design (CAD)

| Category                    | Directory              |
|-----------------------------|------------------------|
| Computer-Aided Design (CAD) | [CAD](./CAD/README.md) |
| Multi-body dynamics         | [MBD](./MBD/README.md) |

### Computer-Aided Design (CAD)

| Name                                                     | Description                                                             | Author                                               | License | Status                              |
|----------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------|---------|-------------------------------------|
| [BRL-CAD](https://brlcad.org/)                           | Mature Constructive Solid Geometry (CSG) CAD system used by US military | U.S. Army Basic Research Laboratories                | GPL     | [FAIL](./CAD/BRL-CAD/README.md)     |
| [VARKON](http://varkon.sourceforge.net/)                 | High-level CAD system                                                   | Orebro university Department of Technology CAD group | LGPL    | [FAIL](./CAD/VARKON/README.md)      |
| [QCad](https://qcad.org/en/qcad)                         | 2-D general CAD system using the Qt widget toolkit                      | RibbonSoft, GmbH                                     | GPL     | [SUCC](./CAD/QCad/README.md)        |
| [Sweet Home 3D](http://sweethome3d.sourceforge.net/)     | Interior design CAD software                                            | eTeks                                                | GPL     | [SUCC](./CAD/SweetHome3D/README.md) |
| [PythonCAD](https://sourceforge.net/projects/pythoncad/) | 2-D general CAD system written in Python                                | matteoboscolo                                        | GPL     | [FAIL](./CAD/PythonCAD/README.md)   |
| [SagCAD](http://sagcad.sourceforge.net/)                 | An easy to use 2D CAD/CAM software                                      | Sagiya Metal Mold Factory, Inc.                      | GPL     | [SUCC](./CAD/SagCAD/README.md)      |
| [Sailcut CAD](http://www.sailcut.com/Sailcut_CAD)        | For designing and visualizing sails                                     | Robert Lainé and Jeremy Lainé                        | GPL     | [SUCC](./CAD/Sailcut_CAD/README.md) |

### Multi-body dynamics

| Name                                 | Description                                   | Author                                                        | License | Status                        |
|--------------------------------------|-----------------------------------------------|---------------------------------------------------------------|---------|-------------------------------|
| [MBDyn](https://www.mbdyn.org/)      | Command-line multi-body dynamics software     | Politecnico di Milano Dipartimento di Ingegneria Aerospaziale | GPL     | [SUCC](./MBD/MBDyn/README.md) |
| [ORSA](http://orsa.sourceforge.net/) | Orbit Reconstruction, Simulation and Analysis | Pasquale Tricarico                                            | GPL     | [FAIL](./MBD/ORSA/README.md)  |


## Data Analysis

| Category      | Directory                                |
|---------------|------------------------------------------|
| Data Analysis | [DataAnalysis](./DataAnalysis/README.md) |

### Data Analysis

| Name                                 | Description                      | Author               | License | Status |
|--------------------------------------|----------------------------------|----------------------|---------|--------|
| [Gpiv](http://gpiv.sourceforge.net/) | Particle Image Velocimetry (PIV) | Gerber van der Graaf | GPL     | CFT    |


## Plumbing & Drain Cleaning

| Category                  | Directory                        |
|---------------------------|----------------------------------|
| Plumbing & Drain Cleaning | [Plumbing](./Plumbing/README.md) |


## Integrated Computational Materials Engineering (ICME)

| Category                                              | Directory                |
|-------------------------------------------------------|--------------------------|
| Integrated Computational Materials Engineering (ICME) | [ICME](./ICME/README.md) |

### Integrated Computational Materials Engineering (ICME)

| Name                              | Description                                                                                 | Author            | License | Status                          |
|-----------------------------------|---------------------------------------------------------------------------------------------|-------------------|---------|---------------------------------|
| [abinit](https://www.abinit.org/) | Density Functional Theory (DFT) for molecules and crystals, including geometry optimization | Xavier Gonze, UCL | GPL     | [FAIL](./ICME/abinit/README.md) |
| [LAMMPS](https://www.lammps.org/) | Parallel molecular dynamics code                                                            |                   | GPL     | [SUCC](./ICME/LAMMPS/README.md) |
