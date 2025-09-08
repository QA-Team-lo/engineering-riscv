### **Guide: Building ParaView on RevyOS (RISC-V)**

There are precompiled ParaView packages for RISC-V in the RevyOS repositories. For those packages, one could attempt to install them with `apt`.

```bash
debian@revyos-lpi4a:~$ apt search paraview
paraview/sid 5.10.1-2+b1 riscv64
  Parallel Visualization Application

paraview-dbgsym/sid 5.10.1-2+b1 riscv64
  debug symbols for paraview

paraview-dev/sid 5.10.1-2+b1 riscv64
  Parallel Visualization Application. Development header files

...
```
However, installation on the test system (RevyOS image 20250420) failed due to unresolved dependencies on obsolete library versions (`libgdal31`, `libtiff5`), necessitating a build from source.

```bash
debian@revyos-lpi4a:~$ sudo apt install paraview
[sudo] password for debian: 
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

Unsatisfied dependencies:
 paraview : Depends: libgdal31 (>= 3.0.0) but it is not installable
            Depends: libtiff5 (>= 4.0.3) but it is not installable
            Recommends: python3-paraview but it is not going to be installed
            Recommends: paraview-doc but it is not going to be installed
Error: Unable to correct problems, you have held broken packages.
debian@revyos-lpi4a:~$ debian@revyos-lpi4a:~$ sudo apt install paraview
[sudo] password for debian: 
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

Unsatisfied dependencies:
 paraview : Depends: libgdal31 (>= 3.0.0) but it is not installable
            Depends: libtiff5 (>= 4.0.3) but it is not installable
            Recommends: python3-paraview but it is not going to be installed
            Recommends: paraview-doc but it is not going to be installed
Error: Unable to correct problems, you have held broken packages.
```

---

This document provides a record of the attempt to build ParaView v6.0.0 from source on a TH1520 Lichee Pi 4A board running RevyOS.

Official Website: [ParaView Homepage](https://www.paraview.org/)

The source code used was from: https://www.paraview.org/files/v6.0/ParaView-v6.0.0.tar.xz

#### Building ParaView v6.0.0 on RISC-V

ParaView is an open-source, multi-platform data analysis and visualization application. This guide details the process undertaken to compile it on a RISC-V platform.

##### Prerequisites

A standard C++ development environment and several libraries are required. The following packages were installed to satisfy the initial dependencies:

```bash
sudo apt-get install build-essential cmake git \
    qtbase5-dev qttools5-dev libqt5x11extras5-dev \
    libgl1-mesa-dev libglu1-mesa-dev \
    python3-dev libopenmpi-dev \
    libhdf5-dev zlib1g-dev
```

##### Build Steps Attempted

The following procedure was used to configure and initiate the build.

1.  **Download and Extract Source:**
    ```bash
    wget https://www.paraview.org/files/v6.0/ParaView-v6.0.0.tar.xz
    tar -xvf ParaView-v6.0.0.tar.xz
    ```

2.  **Configure with CMake:**
    A build directory was created and CMake was run to configure the project.
    ```bash
    cd ParaView-v6.0.0
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=~/paraview-install -DCMAKE_BUILD_TYPE=Release ..
    ```

##### Troubleshooting and Point of Failure

The build process encountered and resolved several issues before halting at a non-recoverable compilation error.

1.  **Missing Qt Modules:** The initial CMake configuration failed due to missing Qt components. These were resolved by installing the corresponding development packages:
    ```bash
    sudo apt install libqt5svg5-dev
    sudo apt install qtxmlpatterns5-dev-tools
    ```

2.  **Final Compilation Error:** The build progressed to approximately 90% completion before failing with a consistent error in the VTK module:
    ```
    /home/debian/ParaView-v6.0.0/VTK/GUISupport/Qt/QVTKOpenGLWindow.cxx:283:43: error: ‘GL_BACK_LEFT’ was not declared in this scope
    /home/debian/ParaView-v6.0.0/VTK/GUISupport/Qt/QVTKOpenGLWindow.cxx:285:43: error: ‘GL_BACK_RIGHT’ was not declared in this scope
    ```

![GL_BACK_LEFT not declared](images/GL_BACK_LEFT_not_declared.png)

##### Analysis and Conclusion

The final error indicates a fundamental incompatibility between the ParaView v6.0.0 source code and the OpenGL environment provided by the Mesa graphics drivers on this RISC-V platform.

The macros `GL_BACK_LEFT` and `GL_BACK_RIGHT` are part of the legacy, full desktop OpenGL specification, specifically for stereoscopic rendering. The graphics drivers on this embedded platform appear to provide a modern OpenGL Core Profile or an OpenGL ES (GLES) implementation, which have deprecated and removed these fixed-functionality macros.

An attempt to resolve this by forcing an EGL-based configuration (`-DVTK_OPENGL_HAS_EGL=ON`) did not alter the outcome.

**The software cannot be built without patching the source code** to remove or conditionally compile the sections that rely on these legacy OpenGL features. This is a known issue when building VTK and ParaView for ARM and other embedded architectures that use modern GLES-based drivers.

##### Further Reading

Developers interested in porting ParaView to this platform should consult the following resources, which discuss similar issues and potential patching strategies:

*   VTK Discourse on building for ARM64 with OpenGL ES:
    [https://discourse.vtk.org/t/building-vtk-for-arm64-with-opengl-es/707/6](https://discourse.vtk.org/t/building-vtk-for-arm64-with-opengl-es/707/6)

*   ParaView Discourse on ARM build failures:
    [https://discourse.paraview.org/t/fail-to-build-paraview-for-arm-arch/6928/2](https://discourse.paraview.org/t/fail-to-build-paraview-for-arm-arch/6928/2)

*   Gentoo bug report discussing similar OpenGL macro issues:
    [https://bugs.gentoo.org/945731](https://bugs.gentoo.org/945731)
