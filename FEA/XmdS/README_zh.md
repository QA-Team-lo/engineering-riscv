### **指南：在 RevyOS 上安装 XmdS**

本文档提供了在运行 RevyOS 的 RISC-V 设备上安装 `XmdS`（用于快速求解微分方程的工具）的说明。`xmds2` 包在 RevyOS 仓库中可用，使安装变得非常简单。

#### 安装

可以使用 `apt` 包管理器执行安装。

```bash
$ sudo apt install xmds2
```

#### 可用性测试

对于以下的程序:

```xmds
<simulation xmds-version="2">
  <name>integer_dimensions</name>
  <author>Graham Dennis</author>
  <description>
    XMDS2 script to test integer dimensions.
  </description>

  <features>
    <benchmark />
    <error_check />
    <bing />
    <diagnostics /> <!-- This will make sure that all nonlocal accesses of dimensions are safe -->
  </features>

  <geometry>
    <propagation_dimension> t </propagation_dimension>
    <transverse_dimensions>
      <dimension name="j" type="integer" lattice="5" domain="(0,4)" />
    </transverse_dimensions>
  </geometry>

  <vector name="main" type="complex">
    <components> x </components>
    <initialisation>
      <![CDATA[
      x = 1.0e-3;
      x(j => 0) = 1.0;
      ]]>
    </initialisation>
  </vector>

  <sequence>
    <integrate algorithm="ARK45" interval="60" steps="25000" tolerance="1.0e-9">
      <samples>1000</samples>
      <operators>
        <integration_vectors>main</integration_vectors>
        <![CDATA[
        long j_minus_one = (j-1) % _lattice_j;
        if (j_minus_one < 0)
          j_minus_one += _lattice_j;
        long j_plus_one  = (j+1) % _lattice_j;
        dx_dt(j => j) = x(j => j)*(x(j => j_minus_one) - x(j => j_plus_one));
        ]]>
      </operators>
    </integrate>
  </sequence>

  <output>
    <sampling_group basis="j" initial_sample="yes">
      <moments>xR</moments>
      <dependencies>main</dependencies>
      <![CDATA[
        xR = x.Re();
      ]]>
    </sampling_group>
  </output>
</simulation>
```

使用 `xmds2` 进行处理:

```bash
debian@revyos-lpi4a:~/xmds$ xmds2 id.xmds
xmds2 version 3.1.0 "My kingdom for a tentacle" (Debian package 3.1.0+dfsg2-3)
Copyright 2000-2019 Graham Dennis, Joseph Hope, Mattias Johnsson
                    and the xmds team
Generating source code...
... done
Compiling simulation...

... done. Type './integer_dimensions' to run.
```

运行输出的程序，结果为:

```
debian@revyos-lpi4a:~/xmds$ ./integer_dimensions

Sampled field (for moment group #1) at t = 2.040000e+00
Current timestep: 3.194309e-02
Sampled field (for moment group #1) at t = 2.100000e+00
Current timestep: 1.947736e-02
Sampled field (for moment group #1) at t = 2.160000e+00
Current timestep: 2.089423e-02
Sampled field (for moment group #1) at t = 2.220000e+00
Current timestep: 2.901261e-02
Sampled field (for moment group #1) at t = 2.280000e+00
Current timestep: 2.483920e-02
Sampled field (for moment group #1) at t = 2.340000e+00
Current timestep: 2.024989e-02
Sampled field (for moment group #1) at t = 2.400000e+00
Current timestep: 1.716860e-02
Sampled field (for moment group #1) at t = 2.460000e+00
Current timestep: 3.244601e-02
Sampled field (for moment group #1) at t = 2.520000e+00
Current timestep: 1.756255e-02
Sampled field (for moment group #1) at t = 2.580000e+00
Current timestep: 2.547378e-02
Sampled field (for moment group #1) at t = 2.640000e+00
Current timestep: 2.350013e-02
Sampled field (for moment group #1) at t = 2.700000e+
Sampled field (for moment group #1) at t = 4.140000e+00
Current timestep: 2.758370e-02
Sampled field (for moment group #1) at t = 4.200000e+00
Current timestep: 1.866039e-02
Sampled field (for moment group #1) at t = 4.260000e+00
Current timestep: 3.034379e-02
Sampled field (for moment group #1) at t = 4.320000e+00
Current timestep: 2.105265e-02
Sampled field (for moment group #1) at t = 4.380000e+00
Current timestep: 2.537075e-02
Sampled field (for moment group #1) at t = 4.440000e+00
Current timestep: 2.241977e-02
Sampled field (for moment group #1) at t = 4.500000e+00
Current timestep: 2.570232e-02
Sampled field (for moment group #1) at t = 4.560000e+00
Current timestep: 2.131451e-02
Sampled field (for moment group #1) at t = 4.620000e+00
Current timestep: 2.745401e-02
Sampled field (for moment group #1) at t = 4.680000e+00
Current timestep: 1.831493e-02
Sampled field (for moment group #1) at t = 4.740000e+00
Current timestep: 3.123612e-02

Sampled field (for moment group #1) at t = 6.900000e+00
Current timestep: 4.162874e-02
Sampled field (for moment group #1) at t = 6.960000e+00
Current timestep: 4.145295e-02
Sampled field (for moment group #1) at t = 7.020000e+00
Current timestep: 4.123988e-02
Sampled field (for moment group #1) at t = 7.080000e+00
Current timestep: 4.113988e-02
Sampled field (for moment group #1) at t = 7.140000e+00
Current timestep: 4.101405e-02
Sampled field (for moment group #1) at t = 7.200000e+00
Current timestep: 4.094344e-02
Sampled field (for moment group #1) at t = 7.260000e+00
Current timestep: 4.083275e-02
Sampled field (for moment group #1) at t = 7.320000e+00
Current timestep: 4.079900e-02
Sampled field (for moment group #1) at t = 7.380000e+00
Current timestep: 4.073901e-02
Sampled field (for moment group #1) at t = 7.440000e+00
Current timestep: 4.072816e-02
Sampled field (for moment group #1) at t = 7.500000e+00
Current timestep: 4.073323e-02

Sampled field (for moment group #1) at t = 9.660000e+00
Current timestep: 3.780861e-02
Sampled field (for moment group #1) at t = 9.720000e+00
Current timestep: 3.780004e-02
Sampled field (for moment group #1) at t = 9.780000e+00
Current timestep: 3.779191e-02
Sampled field (for moment group #1) at t = 9.840000e+00
Current timestep: 3.778836e-02
Sampled field (for moment group #1) at t = 9.900000e+00
Current timestep: 3.778826e-02
Sampled field (for moment group #1) at t = 9.960000e+00
Current timestep: 3.779039e-02
Sampled field (for moment group #1) at t = 1.002000e+01
Current timestep: 3.779255e-02
Sampled field (for moment group #1) at t = 1.008000e+01
Current timestep: 3.780109e-02
Sampled field (for moment group #1) at t = 1.014000e+01
Current timestep: 3.781415e-02
Sampled field (for moment group #1) at t = 1.020000e+01
Current timestep: 3.782236e-02
Sampled field (for moment group #1) at t = 1.026000e+01
Current timestep: 2.223117e-02
Sampled field (for moment group #1) at t = 5.958000e+01
Current timestep: 2.223525e-02
Sampled field (for moment group #1) at t = 5.964000e+01
Current timestep: 2.225582e-02
Sampled field (for moment group #1) at t = 5.970000e+01
Current timestep: 2.225930e-02
Sampled field (for moment group #1) at t = 5.976000e+01
Current timestep: 2.228401e-02
Sampled field (for moment group #1) at t = 5.982000e+01
Current timestep: 2.228837e-02
Sampled field (for moment group #1) at t = 5.988000e+01
Current timestep: 2.231722e-02
Sampled field (for moment group #1) at t = 5.994000e+01
Current timestep: 2.231899e-02
Sampled field (for moment group #1) at t = 6.000000e+01
Current timestep: 1.467195e-02
Segment 1: minimum timestep: 2.400000e-03 maximum timestep: 2.631201e-02
  Attempted 3008 steps, 0.00% steps failed.
Generating output for integer_dimensions
Maximum step error in moment group 1 was 3.195155e-11
Time elapsed for simulation is: 0.07 seconds
```

证明 `xmds2` 在 RevyOS 上有基本可用性
