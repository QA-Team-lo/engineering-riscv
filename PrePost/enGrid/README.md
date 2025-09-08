### **Status Report: Building enGrid on RevyOS (RISC-V)**

This document details the process and outcome of an attempt to build the enGrid mesh generation software on a TH1520 Lichee Pi 4A board running RevyOS (Debian derivative, image dated 20250420).

The source code was obtained from the official repository:
`git clone https://git.code.sf.net/p/engrid/code engrid-code`

#### Summary

The enGrid source code, last updated in 2013, could not be compiled on a modern RISC-V Debian-based system. The primary obstacles are critical dependencies on old obsolete versions of the Qt 4 and VTK 5 libraries, which unavailable in RevyOS package repositories, and a build process reliant on broken, hardcoded scripts. (It refers to download VERY OLD NETGEN 4.9.13 from a URL that is no longer active.)

So, **Cannot be built without significant patches.**

Relavant Documentation: [NETGEN](../NETGEN/README.md) (v6.2.2505 built successfully)

#### Investigation and Build Attempts

The build process was initiated by analyzing the project's structure, which relies on custom shell scripts (`build.bash`, `setup_ubuntu.bash`, no debian) rather than a standard, modern build system.

**1. Dependency Analysis:**
   - Examination of the `setup_ubuntu.bash` and `build.bash` scripts revealed hardcoded dependencies on `qt4-dev-tools` and `libvtk5-qt4-dev`.
   - These packages correspond to **Qt4** and **VTK 5**.
   - The RevyOS (Debian Sid) repositories do not provide Qt4 or VTK 5 packages for the RISC-V architecture. Available versions are Qt5/Qt6 and VTK 6/7/9.

**2. Manual Build Process:**
   - A manual build was attempted by following the logic of the `build.bash` script and substituting modern dependencies (`qtbase5-dev`, `qttools5-dev`, `libvtk7-qt-dev`).
   - The first step of the build script, `scripts/build-nglib.sh`, is responsible for building a bundled version of the Netgen (nglib) library.

**3. Point of Failure:**
   - The `build-nglib.sh` script failed immediately.
   - The script attempts to download the Netgen 4.9.13 source code from a hardcoded URL (`http://engits.eu/files/netgen-4.9.13.zip`).
   - This URL is no longer active, causing the download to fail and halting the entire build process. The script is unable to acquire the source code it needs to compile.

#### Recommendations for Future Attempts

Successfully building enGrid on this platform would require a considerable porting effort. The following actions would be necessary:

1.  **Port from Qt4 to Qt5/Qt6:** The entire codebase would need to be audited and patched to be compatible with the modern Qt5 or Qt6 API. This is a non-trivial task involving changes to headers, class names, and function calls.

2.  **Update VTK API Usage:** The code would need to be updated to work with a modern version of VTK (e.g., VTK 7 or 9), as the VTK 5 API is no longer available.

3.  **Migrate to CMake:** The hardcoded build scripts should be replaced with a CMake configuration. This would involve:
    *   Removing the broken download logic for `nglib`.
    -   Instead, the build system should be configured to find and link against a system-installed version of Netgen (as we successfully built in the previous guide) or use a git submodule for a compatible version.
    *   Properly detecting system versions of Qt and VTK using `find_package`.

Without these patches, the project is not in a buildable state on modern Linux distributions, particularly on the RISC-V architecture.