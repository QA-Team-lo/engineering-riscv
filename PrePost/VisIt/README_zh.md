访问网站：https://visit-dav.github.io/visit-website/releases-as-tables/#series-34

您可以从[这里](https://github.com/visit-dav/visit/releases/download/v3.4.2/visit3.4.2.tar.gz)获取我使用的 VisIt 源代码。

### **状态报告：尝试在 RevyOS (RISC-V) 上构建 VisIt 3.4.2**

本文档详细记录了在运行 RevyOS 的 RISC-V 设备上从源代码构建 VisIt 3.4.2 科学可视化工具的过程和当前状态。由于平台特定问题、过时的构建脚本和源代码与现代系统可用库的不兼容性组合，标准构建过程不可行。

本报告记录了使用系统原生工具（`apt`、`cmake`）的手动构建方法以及遇到的重大挑战，这些挑战目前阻碍了构建的完成。

官方文档：[VisIt 开发者文档](https://visit-sphinx-github-user-manual.readthedocs.io/en/develop/developers/index.html)

#### 先决条件

需要完整的开发环境。初始构建尝试揭示了许多未在文档中明确列出但配置阶段必需的依赖项。

以下命令整合了配置过程中发现的所有依赖项：

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

#### 步骤 1：初始构建尝试（手动 CMake）

提供的构建脚本（`scripts/run-build-visit`）被发现是针对特定 LLNL/LANL 超级计算机硬编码的，在通用 Linux 系统上不可用。因此，使用标准手动 CMake 过程启动构建。

```bash
# 获取并提取源代码
wget https://github.com/visit-dav/visit/releases/download/v3.4.2/visit3.4.2.tar.gz
tar xzf ./visit3.4.2.tar.gz
cd visit3.4.2

# 创建构建目录
mkdir build
cd build
```

#### 步骤 2：克服配置障碍

`cmake` 配置阶段反复失败，需要一系列手动干预和源代码补丁才能继续。每个问题及其解决方案记录如下。

1.  **问题：** 缺少 `config-site` 文件。
    *   **诊断：** 构建系统基于系统的主机名查找机器特定的配置文件，该文件在此平台上不存在。
    *   **解决方案：** 向 `cmake` 命令添加 `-DVISIT_CONFIG_SITE=NONE` 标志。

2.  **问题：** Python 依赖项检查失败。
    *   **诊断：** 构建脚本需要 `pip` 模块可导入，但未将其声明为依赖项。
    *   **解决方案：** 通过 `apt` 手动安装 `python3-pip` 包。

3.  **问题：** 找不到 Zlib 和 HDF5 库，尽管已安装。
    *   **诊断：** 构建脚本（`FindZlib.cmake`、`FindHDF5.cmake`）已损坏。它们忽略标准 CMake 提示变量，并且不搜索标准 RISC-V 库路径（例如 `/usr/lib/riscv64-linux-gnu/hdf5/serial`）。
    *   **解决方案：** 手动编辑两个 `.cmake` 文件，在顶部添加代码块以硬编码库和包含目录的正确路径，后跟 `return()` 命令以绕过损坏的脚本的其余部分。

4.  **问题：** 构建脚本需要 VTK 版本 8.1。
    *   **诊断：** 构建脚本明确搜索 VTK 8.1，该版本已过时且在 Debian Trixie 上不可用。然而，官方 VisIt 3.4 发布说明声明它使用 VTK 9。
    *   **解决方案：** 忽略脚本的要求并安装系统的现代 VTK 9 包（`libvtk9-dev`）。

5.  **问题：** `FindVTK8.cmake` 中的 CMake 语法错误。
    *   **诊断：** 脚本创建 Python 路径列表（`/lib/python3;/lib/python3.13`），但然后在 `if(EXISTS ...)` 命令中使用列表变量，该命令只接受单个路径，导致语法错误。
    *   **解决方案：** 手动编辑 `FindVTK8.cmake`，在将其传递给 `if(EXISTS ...)` 命令之前仅从列表中提取第一个路径，从而修复语法。

应用上述所有修复后，`cmake` 配置阶段现在成功完成。

#### 步骤 3：当前状态 - 编译期间受阻

配置成功后，使用 `make -j$(nproc)` 开始编译。然而，构建目前**受阻**于编译和链接阶段发生的两个关键、未解决的错误。

#### 当前未解决的问题

1.  **致命编译错误：VTK 头文件不兼容**
    *   **问题：** 编译 `/src/vtkqt/vtkQtRenderWindow.C` 时构建失败，因为它找不到头文件 `QVTKOpenGLWidget.h`。
    *   **诊断：** 系统范围搜索（`apt-file search` 和 `find`）确认此头文件在 Debian Trixie for RISC-V 的任何可用 VTK 9 包中**不存在**。这表明 VisIt 3.4.2 源代码与其正在编译的现代 VTK 9 库之间存在基本的 API 不兼容性。原始代码可能是为不同版本或补丁的 VTK 编写的。

2.  **致命链接器错误：缺少 HDF5 库**
    *   **问题：** 多个数据生成工具（例如 `unic`、`xdmf`）在最终链接阶段失败，出现 `undefined reference to 'H5open'` 和其他 HDF5 函数。
    *   **诊断：** 详细构建日志（`make VERBOSE=1`）确认这些特定可执行文件的链接器命令缺少 HDF5 库（`-lhdf5`）。这是位于 `/src/tools/data/datagen/` 中的 `CMakeLists.txt` 文件中的错误，该文件未能链接其使用的库。

### 结论

在此平台上构建 VisIt 3.4.2 的尝试目前**受阻**。虽然通过手动补丁成功克服了许多配置问题，但构建现在面临源代码与系统现代 VTK 9 库的基本不兼容性。（或者也许我们可以将 VTK 8.1（2017）移植到 RevyOS？）

进一步进展需要对 C++ 源代码进行重大修改，以使 VisIt 适应当前的 VTK 9 API，以及进一步调试构建系统的链接器命令。这将任务从复杂的构建过程提升为软件移植工作，超出了本安装指南的范围。