### **指南：在 RevyOS 上构建 GetDP**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 GetDP 软件的说明。

#### 先决条件

可以工作的 C++ 工具链 (`g++`)、`git` 和 `cmake` 是必须的

#### 步骤 1: 获取源代码

从 GetDP 的软件仓库获取源代码

```bash
$ git clone https://gitlab.onelab.info/getdp/getdp.git
```

#### 步骤 2: 使用 CMake 配置构建

执行:

```
$ mkdir build && cd build && cmake .. && make -j"$(nproc)"
```

即可完成构建，执行

```
./getdp
```

运行程序:

```
debian@revyos-lpi4a:~/getdp-git-source/build$ ./getdp

  -res file(s)              load processing results from file(s)
  -name string              use string as generic file name
  -adapt file               read adaptation constraints from file
  -order num                restrict maximum interpolation order
  -cache                    cache network computations to disk
Linear solver options:
  -solver file              specify parameter file (default: solver.par)
  -'Parameter' num          override value of solver parameter 'Parameter'
Output options:
  -bin                      create binary output files
  -v2                       create mesh-based Gmsh output files when possible
Other options:
  -check                    interactive check of problem structure
  -v num                    set verbosity level (default: 5)
  -cpu                      report CPU times for all operations
  -p num                    set progress indicator update (default: 10)
  -onelab name [address]    communicate with ONELAB (file or server address)
  -setnumber name value     set constant number name=value (or -sn)
  -setstring name value     set constant string name=value (or -ss)
  -version                  show version number
  -info                     show detailed version information
  -help                     show this message
```

#### 可用性测试

运行自带的磁静力学示例:

```
debian@revyos-lpi4a:~/getdp-git-source/build$ make test
Running tests...
Test project /home/debian/getdp-git-source/build
    Start 1: ../examples/magnet.pro
1/1 Test #1: ../examples/magnet.pro ...........   Passed    0.81 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.81 sec
```

证明 `getdp` 在 RevyOS 上有基本可用性
