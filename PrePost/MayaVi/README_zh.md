### **指南：使用混合方法在 RevyOS (RISC-V) 上安装 Mayavi**

本文档提供了在现代基于 Debian 的系统（如 Trixie 分支上的 RevyOS）的 RISC-V 设备上从源代码安装 Mayavi 3D 可视化库的说明。标准的 `pip` 或 `uv` 安装失败，因为在 RISC-V 架构上缺乏关键依赖项（如 VTK 和 UI 工具包）的预编译二进制 "wheels"，特别是与新的 Python 版本（例如 3.13）结合使用时。

本指南详细介绍了一种稳健的**混合方法**：使用系统包管理器（`apt`）处理最重的依赖项（VTK），并使用 `uv` 处理 Mayavi 本身、其 UI 后端（`wxPython`）及其纯 Python 依赖项。

官方文档：[Mayavi 安装指南](https://docs.enthought.com/mayavi/mayavi/installation.html)

#### 先决条件

需要可用的开发环境，包括获取源代码的 `git` 和作为 Python 包安装程序的 `uv`。本指南还需要构建工具来编译 `wxPython`（如果二进制 wheels 不可用）。

可以使用以下命令安装所有必需的依赖项：

```bash
sudo apt-get update && sudo apt-get -y install \
    git \
    uv \
    build-essential \
    python3-vtk9 \
    libgtk-3-dev
```
*注意：包含了 `build-essential` 和 `libgtk-3-dev`，因为它们是构建 `wxPython` 的常见要求。*

#### 步骤 1：获取源代码

首先，从 GitHub 克隆官方的 Mayavi 源代码仓库。

```bash
git clone https://github.com/enthought/mayavi.git
cd mayavi
```

#### 步骤 2：创建专门的虚拟环境

为了将系统安装的 VTK 与我们的本地 Python 项目链接，我们必须创建一个可以访问系统 site-packages 的虚拟环境。这是混合方法的关键。

```bash
# 在 'mayavi' 源代码目录内
# 创建 venv，允许它看到系统包
uv venv --system-site-packages

# 激活新的虚拟环境（默认名为 .venv）
source .venv/bin/activate
```
激活后，您的命令提示符将带有 `(.venv)` 前缀。

#### 步骤 3：执行手动、两阶段安装

这是最关键的步骤。标准安装过程由于 Catch-22 情况而失败：依赖解析器和构建脚本都需要 VTK，但无法在标准 Python 包索引（PyPI）中找到它。我们通过手动控制安装分两个阶段来绕过此问题。

1.  **安装 Mayavi 本身，忽略依赖项**

    首先，我们从本地源代码构建并安装 Mayavi。我们使用两个关键标志：
    *   `--no-build-isolation`：强制构建在我们的主虚拟环境中运行，系统 VTK 在其中可见。
    *   `--no-deps`：防止安装程序从 PyPI 检查或安装任何依赖项。

    ```bash
    # 确保 .venv 已激活
    uv pip install --no-build-isolation --no-deps .
    ```
    此命令将成功构建并安装 `mayavi` 包，而不安装其他任何内容。

2.  **手动安装剩余依赖项**

    现在 Mayavi 已安装，我们使用 `uv` 安装其其他依赖项（NumPy、Enthought Tool Suite 和 `wxPython` UI 后端）。这确保它们在虚拟环境中正确注册。

    ```bash
    uv pip install "numpy>=2.0" traits traitsui pyface envisage apptools wxPython
    ```

#### 验证

激活环境后，您可以通过从命令行运行测试脚本来验证完整安装是否正常工作。

```bash
python -c "from mayavi import mlab; print('SUCCESS: Mayavi and its dependencies are fully installed!'); mlab.test_points3d(); mlab.show()"
```

您应该会在终端中看到成功消息，并且应该出现一个新窗口显示交互式 3D 绘图。这确认了 Mayavi、VTK 和 `wxPython` 都正常工作。

![MayaVi With Testing Points](images/mayavi-with-testing-points.png)

### 为什么使用混合方法？

混合方法是必要的，因为 `uv`（或 `pip`）不识别 RISC-V 架构上预编译的 `vtk>=9` 包的可用性。这导致在尝试通过 Python 的包管理器安装 VTK 时安装失败。然而，系统包管理器（`apt`）提供了兼容的 `vtk` 包。通过在虚拟环境中利用 `--system-site-packages` 选项，我们可以直接使用系统安装的 `vtk` 包，绕过对 Python 特定安装的需求。这确保了兼容性并有效解决了依赖项问题。