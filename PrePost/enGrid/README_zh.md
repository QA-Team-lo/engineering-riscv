### **状态报告：在 RevyOS (RISC-V) 上构建 enGrid**

本文档详细说明了在运行 RevyOS（Debian 衍生物，镜像日期 20250420）的 TH1520 Lichee Pi 4A 板上尝试构建 enGrid 网格生成软件的过程和结果。

源代码来自官方仓库：
`git clone https://git.code.sf.net/p/engrid/code engrid-code`

#### 总结

enGrid 源代码（最后更新于 2013 年）无法在现代 RISC-V 基于 Debian 的系统上编译。主要障碍是对旧过时版本的 Qt 4 和 VTK 5 库的关键依赖，这些库在 RevyOS 包仓库中不可用，以及依赖于损坏的硬编码脚本的构建过程。（它引用从不再活跃的 URL 下载非常旧的 NETGEN 4.9.13。）

因此，**无法在没有重大补丁的情况下构建。**

相关文档：[NETGEN](../NETGEN/README.md)（v6.2.2505 构建成功）

#### 调查和构建尝试

通过分析项目结构启动构建过程，该项目依赖于自定义 shell 脚本（`build.bash`、`setup_ubuntu.bash`，没有 debian）而不是标准的现代构建系统。

**1. 依赖项分析：**
   - 检查 `setup_ubuntu.bash` 和 `build.bash` 脚本显示对 `qt4-dev-tools` 和 `libvtk5-qt4-dev` 的硬编码依赖。
   - 这些包对应于 **Qt4** 和 **VTK 5**。
   - RevyOS（Debian Sid）仓库不为 RISC-V 架构提供 Qt4 或 VTK 5 包。可用版本是 Qt5/Qt6 和 VTK 6/7/9。

**2. 手动构建过程：**
   - 通过遵循 `build.bash` 脚本的逻辑并替换现代依赖项（`qtbase5-dev`、`qttools5-dev`、`libvtk7-qt-dev`）尝试手动构建。
   - 构建脚本的第一步 `scripts/build-nglib.sh` 负责构建捆绑版本的 Netgen（nglib）库。

**3. 失败点：**
   - `build-nglib.sh` 脚本立即失败。
   - 脚本尝试从硬编码 URL（`http://engits.eu/files/netgen-4.9.13.zip`）下载 Netgen 4.9.13 源代码。
   - 此 URL 不再活跃，导致下载失败并停止整个构建过程。脚本无法获取编译所需的源代码。

#### 未来尝试的建议

成功在此平台上构建 enGrid 需要相当大的移植工作。以下操作将是必要的：

1.  **从 Qt4 移植到 Qt5/Qt6：** 整个代码库需要审计和修补以与现代 Qt5 或 Qt6 API 兼容。这是一项涉及头文件、类名和函数调用更改的非平凡任务。

2.  **更新 VTK API 使用：** 代码需要更新以与现代版本的 VTK（例如 VTK 7 或 9）一起工作，因为 VTK 5 API 不再可用。

3.  **迁移到 CMake：** 硬编码的构建脚本应该被 CMake 配置替换。这将涉及：
    *   移除 `nglib` 的损坏下载逻辑。
    -   相反，构建系统应该配置为查找并链接到系统安装的 Netgen 版本（如我们在之前的指南中成功构建的）或使用 git 子模块获取兼容版本。
    *   使用 `find_package` 正确检测系统版本的 Qt 和 VTK。

没有这些补丁，项目在现代 Linux 发行版上，特别是在 RISC-V 架构上，不是可构建状态。
