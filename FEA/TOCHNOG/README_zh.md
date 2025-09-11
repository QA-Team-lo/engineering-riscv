### **指南：在 RevyOS 上构建 tochnog**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `tochnog` 软件的说明。

#### 步骤 1: 获取源代码

从 `tochnog` 的官网获取源代码: https://sourceforge.net/projects/tochnog/

#### 步骤 2: 更新构建系统并编译

由于 `tochnog` 的构建系统太老，无法识别到 `riscv64` 这个架构，故需要手动更新两个文件
```
debian@revyos-lpi4a:~/tochnog/tochnog$ ./configure
checking build system type... ./config.guess: unable to guess system type

config.guess timestamp = 2002-01-02

uname -m = riscv64
uname -r = 6.6.92-th1520
uname -s = Linux
uname -v = #2025.05.26.14.02+c9a17b235 SMP Mon May 26 14:22:33 UTC 2025

/usr/bin/uname -p = unknown
/bin/uname -X     =

hostinfo               =
/bin/universe          =
/usr/bin/arch -k       =
/bin/arch              = riscv64
/usr/bin/oslevel       =
/usr/convex/getsysinfo =

UNAME_MACHINE = riscv64
UNAME_RELEASE = 6.6.92-th1520
UNAME_SYSTEM  = Linux
UNAME_VERSION = #2025.05.26.14.02+c9a17b235 SMP Mon May 26 14:22:33 UTC 2025
configure: error: cannot guess build type; you must specify one
```

在终端执行:
```
$ wget -O config.guess https://git.savannah.gnu.org/cgit/config.git/plain/config.guess
$ wget -O config.sub   https://git.savannah.gnu.org/cgit/config.git/plain/config.sub
$ chmod +x config.guess config.sub
$ ./configure
```

然后再编译:
```
$ make
$ sudo make install
```

#### 可用性测试

```
debian@revyos-lpi4a:~/tochnog/tochnog$ cat test/examp1.dat
(Example 1. See manual. )
group_type 0  -materi
group_materi_density 0  1.
group_materi_viscosity 0  1.e-2
group_materi_elasti_compressibility 0  1.e-2
group_materi_memory 0  -updated_without_rotation
group_integration_points 0  -minimal

post_point 1   0.8 0.1
print_filter 0  -post_point_dof -all -velx -vely

control_mesh_refine_globally           3  -h_refinement
control_mesh_refine_globally           4  -h_refinement

control_timestep                      10  0.01 1.
control_timestep_iterations           10  2
(
control_print                        10  -time_current -post_point_dof
)

target_item 0  -post_point_dof 1 -velx
target_value 0   -0.041 0.1
end_data
debian@revyos-lpi4a:~/tochnog/tochnog$ tochnog test/examp1.dat
CPU time: 2 s
```

证明 `tochnog` 在 RevyOS 上具有基本可用性
