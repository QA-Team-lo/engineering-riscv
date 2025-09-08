### **Guide: Compiling and Installing FiPy on RevyOS**

This document provides instructions for installing the `FiPy` scientific computing package on a RISC-V device running the RevyOS Debian derivative. The process requires compiling `FiPy` and its dependencies from source, and the compilation is managed by `uv`. This guide details how to resolve the specific compilation errors that arise from missing system-level libraries.

#### **1. Environment and Hardware**

*   **Hardware Platform**: Lichee Pi 4A (LPI4A)
*   **System Architecture**: RISC-V (`riscv64`)
*   **Operating System**: RevyOS (Debian Sid based)
*   **Python Environment Manager**: `uv`

#### **2. The Complete Prerequisite Installation**

To avoid encountering errors sequentially, it is best to install all required system-level development libraries and compilers beforehand. These are essential for building the C, C++, and Fortran components of Python's scientific stack.

Execute the following command to install all necessary prerequisites:

```bash
sudo apt update
sudo apt install build-essential gfortran python3-dev libopenblas-dev libjpeg-dev zlib1g-dev libtiff5-dev libfreetype-dev
```

*   **`build-essential`**: Provides the fundamental C/C++ compilers (`gcc`, `g++`) and `make` utility.
*   **`gfortran`**: The GNU Fortran compiler, a mandatory requirement for compiling `SciPy`.
*   **`python3-dev`**: Provides the `Python.h` header files needed to build any Python C-extension.
*   **`libopenblas-dev`**: Provides a highly optimized BLAS/LAPACK library for high-performance linear algebra, required by `SciPy` and `NumPy`.
*   **`libjpeg-dev`, `zlib1g-dev`, `libtiff5-dev`, `libfreetype-dev`**: Provide development headers for various image and font formats, required by `Pillow`, a dependency of `Matplotlib`.

#### **3. Installation Procedure**

**Step 3.1: Install the `uv` Tool**
`uv` is a fast Python package installer that will manage the environment and compilation/installation process.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
After installation, follow the on-screen instructions to add `uv` to your shell's PATH, then restart your terminal or source your profile (`source ~/.bashrc`).

**Step 3.2: Set Up the Project Environment**
Create a dedicated directory and a virtual environment for the project.

```bash
mkdir my-fipy-project
cd my-fipy-project

# Create and activate the virtual environment
uv venv
source .venv/bin/activate
```

**Step 3.3: Install FiPy**
Begin the installation. `uv` will now attempt to download and compile `FiPy` and all its dependencies automatically.

```bash
uv pip install fipy
```
If you skipped the complete prerequisite installation in Section 2, you will encounter a series of errors. The next section details these errors and their specific solutions.

#### **4. Troubleshooting Guide: Matching Errors to Solutions**

This section is a direct reference for resolving compilation failures. If you did not install all prerequisites, you will likely encounter these errors in order.

**Error 1: Missing `Python.h`**
*   **Error Message**: `fatal error: Python.h: No such file or directory`
*   **Context**: This occurs when compiling a package with C extensions (like `kiwisolver`) and the Python development headers are not found.
*   **Solution**: Install the `python3-dev` package.
    ```bash
    sudo apt install python3-dev
    ```

**Error 2: Missing `jpeg` library**
*   **Error Message**: `RequiredDependencyException: The headers or library files could not be found for jpeg`
*   **Context**: This occurs when compiling `Pillow`, which needs the `libjpeg` library to provide JPEG image support.
*   **Solution**: Install the `libjpeg-dev` package.
    ```bash
    sudo apt install libjpeg-dev
    ```

**Error 3: `apt` Dependency Conflict for `libfreetype`**
*   **Error Message**: `Unsatisfied dependencies: libfreetype6-dev : Depends: libfreetype-dev (= 2.12.1+dfsg-5) but 2.13.3+dfsg-1 is to be installed`
*   **Context**: The system's package sources have a version conflict, preventing the installation of `libfreetype6-dev`.
*   **Solution**: Bypass the problematic meta-package and install the core development library directly, which is what `Pillow` actually requires.
    ```bash
    sudo apt install libfreetype-dev
    ```

**Error 4: Missing Fortran Compiler**
*   **Error Message**: `ERROR: Unknown compiler(s): [['gfortran'], ...]` followed by `No such file or directory: 'gfortran'`
*   **Context**: This occurs when the `SciPy` build system cannot find a Fortran compiler.
*   **Solution**: Install the GNU Fortran compiler.
    ```bash
    sudo apt install gfortran
    ```

**Error 5: Missing `OpenBLAS` Library**
*   **Error Message**: `ERROR: Dependency "OpenBLAS" not found, tried pkgconfig`
*   **Context**: This occurs during the `SciPy` build when it cannot find a required high-performance linear algebra library.
*   **Solution**: Install the OpenBLAS development package.
    ```bash
    sudo apt install libopenblas-dev
    ```

#### **5. Final Verification**

After the `uv pip install fipy` command completes successfully, verify the installation with a test script.

**Step 5.1: Create `test_fipy.py`**
Create a file with the following Python code:

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

**Step 5.2: Run the Test**
Execute the script from within your activated virtual environment.

```bash
python test_fipy.py
```

**Expected Output:**
A successful installation will produce the following output, confirming that `FiPy` is fully functional.

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