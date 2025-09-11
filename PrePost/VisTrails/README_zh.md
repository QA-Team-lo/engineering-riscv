### **状态报告：在 RevyOS (RISC-V) 上执行 VisTrails**

#### 目标

本报告记录了在使用 Python 3.13 环境的 RevyOS 系统上安装和尝试运行 `vistrails` 包的过程。该包安装成功但在运行时失败。

#### 1. 成功的安装步骤

使用 `uv` 和系统的原生 Python 3.13 解释器创建了虚拟环境。执行了命令 `uv pip install vistrails` 并成功完成。

#### 2. 观察到的运行时失败

尝试使用 `vistrails` 命令启动应用程序时，程序立即终止。产生了以下 `SyntaxError`：

```log
Traceback (most recent call last):
  File "/home/debian/vistrials/.venv/bin/vistrails", line 4, in <module>
    from vistrails.run import main
  File "/home/debian/vistrials/.venv/lib/python3.13/site-packages/vistrails/__init__.py", line 58, in <module>
    from vistrails.core.api import *
  File "/home/debian/vistrials/.venv/lib/python3.13/site-packages/vistrails/core/api.py", line 155
    vistrail.add_action(action, 0L)
                                ^
SyntaxError: invalid decimal literal
```

#### 3. 失败分析

错误直接指向代码 `0L`。`L` 后缀用于整数是 Python 2 特定的语法。此语法在 Python 3 中已移除。因此 Python 3.13 解释器无法解析此代码，导致 `SyntaxError`。这表明 VisTrails 源代码是为 Python 2 运行时编写的。

#### 4. 系统环境上下文

检查系统的包管理器（`apt search python2`）确认在此 RISC-V 平台的标准软件仓库中不可用 Python 2 运行时。

#### 结论

由于在其源代码中使用 Python 2 特定语法，`vistrails` 包无法在 Python 3 环境中运行。在此系统上缺乏现成可用的 Python 2 环境阻碍了直接解决方案。

#### 建议

提供一个包含兼容 Python 2 运行时的自包含 VisTrails 构建将是一个很好的解决方案。这将使应用程序能够在现代平台（如 RISC-V）上运行，这些平台上没有系统级 Python 2 安装。