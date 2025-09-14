Visit Website: https://visit-dav.github.io/visit-website/releases-as-tables/#series-34

You could get Visit source code I used from [here](https://github.com/visit-dav/visit/releases/download/v3.4.2/visit3.4.2.tar.gz)


### **Status Report: Attempting to Build VisIt 3.4.2 on RevyOS (RISC-V)**

This document details the process and current status of attempting to build the VisIt 3.4.2 scientific visualization tool from source on a RISC-V device running RevyOS. The standard build process is not viable due to a combination of platform-specific issues, outdated build scripts, and source code incompatibilities with the modern libraries available on this system.

This report documents a manual build approach using the system's native tools (`apt`, `cmake`) and the significant challenges encountered, which currently block the completion of the build.

Official documentation: [VisIt Developer Docs](https://visit-sphinx-github-user-manual.readthedocs.io/en/develop/developers/index.html)

#### Prerequisites

A full development environment is required. The initial build attempt revealed numerous dependencies that are not explicitly listed in the documentation but are necessary for the configuration phase.

The following command consolidates all dependencies discovered during the configuration process:

```bash
sudo apt-get update && sudo apt-get -y install \
    build-essential \
    cmake-curses-gui \
    python3-dev \
    python3-pip \
    python3-setuptools \
    qtbase5-dev \
    qttools5-dev \
    libqt5opengl5-dev \
    qtdeclarative5-dev \
    libqt5x11extras5-dev \
    libx11-dev \
    libxt-dev \
    libglu1-mesa-dev \
    libhdf5-dev \
    zlib1g-dev \
    libvtk9-dev
```

#### Step 1: Initial Build Attempt (Manual CMake)

The provided build script (`scripts/run-build-visit`) was found to be hard-coded for specific LLNL/LANL supercomputers and is not usable on a generic Linux system. Therefore, the build was initiated using the standard manual CMake process.

```bash
# Obtain and extract the source code
wget https://github.com/visit-dav/visit/releases/download/v3.4.2/visit3.4.2.tar.gz
tar xzf ./visit3.4.2.tar.gz
cd visit3.4.2

# Create a build directory
mkdir build
cd build
```

#### Step 2: Overcoming Configuration Hurdles

The `cmake` configuration phase failed repeatedly, requiring a series of manual interventions and source code patches to proceed. Each issue and its solution is documented below.

1.  **Problem:** Missing `config-site` file.
    *   **Diagnosis:** The build system looks for a machine-specific configuration file based on the system's hostname, which does not exist for this platform.
    *   **Solution:** Added the `-DVISIT_CONFIG_SITE=NONE` flag to the `cmake` command.

2.  **Problem:** Python dependency checks failed.
    *   **Diagnosis:** The build script requires the `pip` module to be importable but does not declare it as a dependency.
    *   **Solution:** Manually installed the `python3-pip` package via `apt`.

3.  **Problem:** Zlib and HDF5 libraries not found, despite being installed.
    *   **Diagnosis:** The build scripts (`FindZlib.cmake`, `FindHDF5.cmake`) are broken. They ignore standard CMake hint variables and do not search standard RISC-V library paths (e.g., `/usr/lib/riscv64-linux-gnu/hdf5/serial`).
    *   **Solution:** Manually edited both `.cmake` files, adding a block of code at the top to hard-code the correct paths to the library and include directories, followed by a `return()` command to bypass the rest of the broken script.

4.  **Problem:** Build script requires VTK version 8.1.
    *   **Diagnosis:** The build script explicitly searches for VTK 8.1, which is obsolete and unavailable on Debian Trixie. However, the official VisIt 3.4 release notes state it uses VTK 9.
    *   **Solution:** Ignored the script's requirement and installed the system's modern VTK 9 package (`libvtk9-dev`).

5.  **Problem:** CMake syntax error in `FindVTK8.cmake`.
    *   **Diagnosis:** The script creates a list of Python paths (`/lib/python3;/lib/python3.13`) but then uses the list variable in an `if(EXISTS ...)` command, which only accepts a single path, causing a syntax error.
    *   **Solution:** Manually edited `FindVTK8.cmake` to extract only the first path from the list before passing it to the `if(EXISTS ...)` command, thus fixing the syntax.

After applying all of the above fixes, the `cmake` configuration phase now completes successfully.

#### Step 3: Current Status - Blocked During Compilation

With a successful configuration, the compilation was started using `make -j$(nproc)`. However, the build is currently **blocked** by two critical, unresolved errors that occur during the compilation and linking phase.

#### Current Unresolved Issues

1.  **Fatal Compilation Error: VTK Header Incompatibility**
    *   **Problem:** The build fails when compiling `/src/vtkqt/vtkQtRenderWindow.C` because it cannot find the header file `QVTKOpenGLWidget.h`.
    *   **Diagnosis:** A system-wide search (`apt-file search` and `find`) confirms that this header file **does not exist** in any available VTK 9 package on Debian Trixie for RISC-V. This indicates a fundamental API incompatibility between the VisIt 3.4.2 source code and the modern VTK 9 library it is being compiled against. The original code was likely written for a different version or patch of VTK.

2.  **Fatal Linker Error: Missing HDF5 Library**
    *   **Problem:** Multiple data generation tools (e.g., `unic`, `xdmf`) fail at the final linking stage with `undefined reference to 'H5open'` and other HDF5 functions.
    *   **Diagnosis:** A verbose build log (`make VERBOSE=1`) confirms that the linker command for these specific executables is missing the HDF5 library (`-lhdf5`). This is a bug in the `CMakeLists.txt` file located in `/src/tools/data/datagen/`, which fails to link the libraries it uses.

### Conclusion

The attempt to build VisIt 3.4.2 on this platform is currently **blocked**. While numerous configuration issues were successfully overcome through manual patching, the build now faces fundamental source code incompatibilities with the system's modern VTK 9 library. (Or maybe we could port VTK 8.1 (2017) to RevyOS?)

Further progress would require significant C++ source code modifications to adapt VisIt to the current VTK 9 API, as well as further debugging of the build system's linker commands. This elevates the task from a complex build process to a software porting effort, which is beyond the scope of this installation guide.