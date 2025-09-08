### **指南：在 RevyOS 上编译和安装 FiPy**

本文档提供了在运行 RevyOS Debian 衍生物的 RISC-V 设备上安装 `FiPy` 科学计算包的说明。该过程需要从源代码编译 `FiPy` 及其依赖项，编译由 `uv` 管理。本指南详细说明了如何解决因缺少系统级库而产生的特定编译错误。

#### **1. 环境和硬件**

*   **硬件平台**: Lichee Pi 4A (LPI4A)
*   **系统架构**: RISC-V (`riscv64`)
*   **操作系统**: RevyOS (基于 Debian Sid)
*   **Python 环境管理器**: `uv`

#### **2. 完整的先决条件安装**

为了避免顺序遇到错误，最好事先安装所有必需的系统级开发库和编译器。这些对于构建 Python 科学栈的 C、C++ 和 Fortran 组件至关重要。

执行以下命令安装所有必要的先决条件：

```bash
sudo apt update
sudo apt install build-essential gfortran python3-dev libopenblas-dev libjpeg-dev zlib1g-dev libtiff5-dev libfreetype-dev
```

*   **`build-essential`**: 提供基本的 C/C++ 编译器 (`gcc`, `g++`) 和 `make` 实用程序。
*   **`gfortran`**: GNU Fortran 编译器，编译 `SciPy` 的强制要求。
*   **`python3-dev`**: 提供构建任何 Python C 扩展所需的 `Python.h` 头文件。
*   **`libopenblas-dev`**: 提供高性能线性代数的高度优化 BLAS/LAPACK 库，`SciPy` 和 `NumPy` 需要。
*   **`libjpeg-dev`, `zlib1g-dev`, `libtiff5-dev`, `libfreetype-dev`**: 提供各种图像和字体格式的开发头文件，`Matplotlib` 的依赖项 `Pillow` 需要。

#### **3. 安装程序**

**步骤 3.1: 安装 `uv` 工具**
`uv` 是一个快速的 Python 包安装器，将管理环境和编译/安装过程。

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
安装后，按照屏幕上的说明将 `uv` 添加到 shell 的 PATH 中，然后重新启动终端或加载配置文件 (`source ~/.bashrc`)。

**步骤 3.2: 设置项目环境**
创建专用目录和项目的虚拟环境。

```bash
mkdir my-fipy-project
cd my-fipy-project

# 创建并激活虚拟环境
uv venv
source .venv/bin/activate
```

**步骤 3.3: 安装 FiPy**
开始安装。`uv` 现在将尝试自动下载和编译 `FiPy` 及其所有依赖项。

```bash
uv pip install fipy
```
如果您跳过了第 2 节中的完整先决条件安装，您将遇到一系列错误。下一节详细说明了这些错误及其具体解决方案。

#### **4. 故障排除指南：将错误与解决方案匹配**

本节是解决编译失败的直接参考。如果您没有安装所有先决条件，您可能会按顺序遇到这些错误。

**错误 1: 缺少 `Python.h`**
*   **错误消息**: `fatal error: Python.h: No such file or directory`
*   **上下文**: 当编译带有 C 扩展的包（如 `kiwisolver`）且找不到 Python 开发头文件时发生。
*   **解决方案**: 安装 `python3-dev` 包。
    ```bash
    sudo apt install python3-dev
    ```

**错误 2: 缺少 `jpeg` 库**
*   **错误消息**: `RequiredDependencyException: The headers or library files could not be found for jpeg`
*   **上下文**: 当编译 `Pillow` 时发生，它需要 `libjpeg` 库来提供 JPEG 图像支持。
*   **解决方案**: 安装 `libjpeg-dev` 包。
    ```bash
    sudo apt install libjpeg-dev
    ```

**错误 3: `apt` 对 `libfreetype` 的依赖冲突**
*   **错误消息**: `Unsatisfied dependencies: libfreetype6-dev : Depends: libfreetype-dev (= 2.12.1+dfsg-5) but 2.13.3+dfsg-1 is to be installed`
*   **上下文**: 系统的包源存在版本冲突，阻止安装 `libfreetype6-dev`。
*   **解决方案**: 绕过有问题的元包，直接安装核心开发库，这是 `Pillow` 实际需要的。
    ```bash
    sudo apt install libfreetype-dev
    ```

**错误 4: 缺少 Fortran 编译器**
*   **错误消息**: `ERROR: Unknown compiler(s): [['gfortran'], ...]` 后跟 `No such file or directory: 'gfortran'`
*   **上下文**: 当 `SciPy` 构建系统找不到 Fortran 编译器时发生。
*   **解决方案**: 安装 GNU Fortran 编译器。
    ```bash
    sudo apt install gfortran
    ```

**错误 5: 缺少 `OpenBLAS` 库**
*   **错误消息**: `ERROR: Dependency "OpenBLAS" not found, tried pkgconfig`
*   **上下文**: 在 `SciPy` 构建期间发生，当它找不到所需的高性能线性代数库时。
*   **解决方案**: 安装 OpenBLAS 开发包。
    ```bash
    sudo apt install libopenblas-dev
    ```

#### **5. 最终验证**

在 `uv pip install fipy` 命令成功完成后，使用测试脚本验证安装。

**步骤 5.1: 创建 `test_fipy.py`**
创建包含以下 Python 代码的文件：

```python
# test_fipy.py
import fipy as fp

print("FiPy Test: Solving a 1D steady-state diffusion problem...")

# 1. Create a mesh
mesh = fp.Grid1D(nx=50, dx=0.02)
print("-> Mesh created successfully.")

# 2. Define a cell variable
phi = fp.CellVariable(name="solution variable", mesh=mesh, value=0.)

# 3. Apply boundary conditions
phi.constrain(0., where=mesh.facesLeft)
phi.constrain(1., where=mesh.facesRight)
print("-> Boundary conditions applied successfully.")

# 4. Define the equation
equation = fp.DiffusionTerm(coeff=1.0) == 0
print("-> Equation defined successfully.")

# 5. Solve
equation.solve(var=phi)
print("-> Equation solved successfully.")

# 6. Verify results
print("\n----- Verification -----")
print("Solution values for the first 5 cells:")
print(phi.value[:5])
print("\nFiPy installation appears to be working correctly!")
```

**步骤 5.2: 运行测试**
在激活的虚拟环境中执行脚本。

```bash
python test_fipy.py
```

**预期输出：**
成功安装将产生以下输出，确认 `FiPy` 完全功能正常。

```
FiPy Test: Solving a 1D steady-state diffusion problem...
-> Mesh created successfully.
-> Boundary conditions applied successfully.
-> Equation defined successfully.
-> Equation solved successfully.

----- Verification -----
Solution values for the first 5 cells:
[0.01 0.03 0.05 0.07 0.09]

FiPy installation appears to be working correctly!
```
