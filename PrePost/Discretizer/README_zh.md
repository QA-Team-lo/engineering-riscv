您可以从[这里](https://sourceforge.net/projects/discretizer/files/discretizer/alpha-one/discretizer-alpha.zip/download)下载最新的 Discretizer 源代码 zip 文件。

![需要 OpenGL 扩展](images/01-need_opengl_extensions.png)

### **状态报告：在 RevyOS (RISC-V) 上运行 Discretizer**

本文档详细记录了在运行 RevyOS（Debian 衍生版）的 RISC-V 板上尝试运行 Discretizer 网格生成软件的过程和结果。

源代码从官方的 SourceForge 项目页面获取。虽然该项目的最新更新是 2010 年的 Windows 标记版本，但最新的可用源代码来自 2008-06-15 的 alpha 版本：
`https://sourceforge.net/projects/discretizer/files/discretizer/alpha-one/discretizer-alpha.zip/download`

#### 总结

使用其 2008 年的最新可用源代码，Discretizer 软件在现代 RISC-V Debian 系统上**部分功能可用**。该应用程序使用 Ruby 编写，需要大量补丁来克服其目标版本（Ruby ~1.8）与现代 Ruby 3.x 环境之间的依赖关系和语法问题。

在解决所有脚本级错误后，应用程序成功启动 GUI 窗口。然而，它立即显示一个致命错误，指出缺少其 "OpenGL extension"。调查发现，核心问题在于其 GUI 工具包 gem `fxruby` 无法在 RISC-V 平台上加载其原生 OpenGL 组件（`fox16/glshapes`），即使所有系统依赖项都已正确安装。

因此，最终状态是：**部分功能可用，但核心 3D 可视化组件无法初始化。若不深入调试其原生 GUI 依赖项（`fxruby`）在 RISC-V 架构上的问题，则无法使其完全功能化。**

#### 调查和执行过程

该过程涉及逐步调试周期以解决一系列兼容性问题。

**1. 初步分析：**
   - 该项目被识别为 Ruby 脚本集合，而不是需要编译的 C/C++ 项目。主要入口点是 `discretizer.rb`。

**2. 解决 Ruby 环境和路径问题：**
   - **问题：** 初始执行失败，出现 `LoadError: cannot load such file -- tableio`。
   - **原因：** 现代 Ruby (1.9+) 出于安全原因，不再默认将当前目录（`.`）包含在其加载路径中。
   - **解决方案：** 使用 `ruby -I. discretizer.rb` 手动将当前目录添加到加载路径中启动应用程序。

**3. 解决缺失依赖项：**
   - **问题：** 后续错误指示缺少库，如 `fox16`、`mathn` 和 `shell`。
   - **原因：** `fox16` 对应于 `fxruby` GUI gem，未安装。`mathn` 和 `shell` 是旧版 Ruby 中的标准库，但在现代 Ruby 中已被提取到单独的 gems 中。
   - **解决方案：** 安装了所有必需的系统级库（`libfox-1.6-dev`、`libgl1-mesa-dev` 等）和 Ruby gems：`sudo gem install fxruby mathn shell`。

**4. 解决 Ruby 语法不兼容性：**
   - **问题：** 应用程序失败，出现 `SyntaxError: Invalid break` 在多个文件中。
   - **原因：** 代码库在循环外使用了 `break`，这在 Ruby 1.8 中是允许的，但在现代 Ruby 中是非法的。
   - **解决方案：** 使用 `find` 命令结合 `awk` 脚本智能替换所有 `.rb` 文件中的无效 `break` 语句。脚本尝试跟踪循环深度，仅在 `break` 出现在循环结构外时将其替换为 `return`。执行的确切命令如下：
     ```bash
     find . -type f -name "*.rb" -print0 | while IFS= read -r -d $'\0' file; do
         awk '
         /\b(while|for|until|loop|do)\b|\.each\b/ { loop_depth++ }
         /^\s*end\s*$/ { if (loop_depth > 0) loop_depth-- }
         /^\s*break\s*$/ && loop_depth == 0 { sub("break", "return"); }
         { print }
         ' "$file" > "$file.tmp" && mv "$file.tmp" "$file"
     done
     ```

**5. 失败点：**
   - 在完成上述所有修复后，运行 `ruby -I. discretizer.rb` 不再在终端中产生脚本错误。相反，它成功启动了一个图形对话框，并显示错误消息：“抱歉，此示例依赖于 OpenGL 扩展。”
   - 分析 `discretizer.rb` 源代码确认此对话框是由 `rescue LoadError` 块触发的，当行 `require 'fox16/glshapes'` 失败时。这表明 `fxruby` gem 存在，但其处理 OpenGL 画布的特定模块无法加载。
   - 最后尝试通过假设 `fxruby` 安装不完整（即在安装 OpenGL 开发库之前安装）来解决此问题。执行了完整的重新安装：`sudo gem uninstall fxruby`，然后是 `sudo gem install fxruby`。
   - 此操作未改变结果。应用程序仍然在完全相同的点失败，并显示相同的错误消息。

#### 结论

Discretizer 应用程序的所有脚本级不兼容性已成功解决。最终的关键阻碍是其依赖项 `fxruby` gem 中的原生组件加载失败。

`fxruby` gem 的 `fox16/glshapes` 模块将 FOX 工具包链接到系统的 OpenGL 库，但在此特定 RISC-V 平台上无法加载。尽管拥有完全兼容的 OpenGL 4.5 环境（通过 `glxinfo` 确认），但此旧版 gem 的原生 C++ 代码与现代 RISC-V 图形栈之间存在深层不兼容性。

解决此问题需要调试 `fxruby` gem 的 C/C++ 源代码，以确定其在 RISC-V 上加载失败的原因，这是一项超出简单应用程序修补范围的移植工作。