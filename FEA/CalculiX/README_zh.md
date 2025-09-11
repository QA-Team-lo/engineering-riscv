### **指南：在 RevyOS 上构建 CalculiX**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `CalculiX` 软件的说明。

#### 步骤 1: 安装依赖

使用 `apt` 安装编译所需要的软件包:

```bash
$ sudo apt update
$ sudo apt install build-essential gfortran make libarpack2-dev libopenblas-dev
```

#### 步骤 2: 获取源代码

从 `CalculiX` 官方网站下载 `ccx` 的源码，然后解压:
```
$ wget https://www.dhondt.de/ccx_2.22.src.tar.bz2
$ sudo tar -xvjf ~/ccx_2.22.src.tar.bz2
```

#### 步骤 3: 获取并编译 SPOOLES 2.2

`CalculiX` 默认用特定版本的 `SPOOLES` 做稀疏矩阵求解，故需要手动编译

首先获取 SPOOLES 2.2 的源码:

```bash
$ cd /usr/local && sudo mkdir spooles && cd spooles
$ sudo wget http://netlib.org/linalg/spooles/spooles.2.2.tgz
$ sudo tar -xvzf spooles.2.2.tgz
```

随后修改 SPOOLES 的源码，使其能正常编译:

```bash
$ sudo sed -i 's/drawTree\.c/draw.c/' Tree/src/makeGlobalLib
$ sudo find . -type f -name '*.c' -exec sed -i -E 's/IVinit\(([^,]+),[[:space:]]*NULL\)/IVinit(\1, 0)/g' {} +
```

然后构建:

```bash
$ sudo make global
```

完成后会生成 `spooles.a`:

```
/usr/local/spooles/spooles.a
```

#### 步骤 4: 修改 CalculiX 的 Makefile 并编译

回到 `CalculiX` 的源码目录，修改 Makefile 为:

```bash
debian@revyos-lpi4a:~/CalculiX/ccx_2.22/src$ cat Makefile

include Makefile.inc

SCCXMAIN = ccx_2.22.c

OCCXF = $(SCCXF:.f=.o)
OCCXC = $(SCCXC:.c=.o)
OCCXMAIN = $(SCCXMAIN:.c=.o)

DIR=/usr/local/spooles.2.2

LIBS = \
       $(DIR)/spooles.a \
        -larpack -lopenblas \
        -lpthread -lm -lc

ccx_2.22: $(OCCXMAIN) ccx_2.22.a  $(LIBS)
        ./date.pl; $(CC) $(CFLAGS) -c ccx_2.22.c; $(FC)  -Wall -O2 -o $@ $(OCCXMAIN) ccx_2.22.a $(LIBS) -fopenmp

ccx_2.22.a: $(OCCXF) $(OCCXC)

        ar vr $@ $?
```

接下来就可以编译了:

```
$ make
```

编译完成后，可以在当前目录找到 `./ccx_2.22` 这个可执行文件

#### 可用性测试

对于以下测试程序:

```
**
**   Structure: cube.
**   Test objective: equations with 2 terms.
**
*HEADING
Model: balk
*NODE
1,0.,0.,0.
2,1.,0.,0.
3,1.,1.,0.
4,0.,1.,0.
5,0.,0.,1.
6,1.,0.,1.
7,1.,1.,1.
8,0.,1.,1.
9,.25,0.,0.
10,.5,0.,0.
11,.75,0.,0.
12,1.,.25,0.
13,1.,.5,0.
14,1.,.75,0.
15,.75,1.,0.
16,.5,1.,0.
17,.25,1.,0.
18,0.,.75,0.
19,0.,.5,0.
20,0.,.25,0.
21,.25,0.,1.
22,.5,0.,1.
23,.75,0.,1.
24,1.,.25,1.
25,1.,.5,1.
26,1.,.75,1.
27,.75,1.,1.
28,.5,1.,1.
29,.25,1.,1.
30,0.,.75,1.
31,0.,.5,1.
32,0.,.25,1.
33,1.,0.,.25
34,1.,0.,.5
35,1.,0.,.75
36,0.,0.,.75
37,0.,0.,.5
38,0.,0.,.25
39,1.,1.,.25
40,1.,1.,.5
41,1.,1.,.75
42,0.,1.,.25
43,0.,1.,.5
44,0.,1.,.75
45,.5,.25,0.
46,.25,.5,0.
47,.5,.5,0.
48,.75,.5,0.
49,.5,.75,0.
50,.5,.25,1.
51,.25,.5,1.
52,.5,.5,1.
53,.75,.5,1.
54,.5,.75,1.
55,.5,0.,.25
56,.25,0.,.5
57,.5,0.,.5
58,.75,0.,.5
59,.5,0.,.75
60,1.,.5,.25
61,1.,.25,.5
62,1.,.5,.5
63,1.,.75,.5
64,1.,.5,.75
65,.5,1.,.25
66,.75,1.,.5
67,.5,1.,.5
68,.25,1.,.5
69,.5,1.,.75
70,0.,.5,.25
71,0.,.75,.5
72,0.,.5,.5
73,0.,.25,.5
74,0.,.5,.75
75,.5,.5,.25
76,.5,.25,.5
77,.25,.5,.5
78,.5,.5,.5
79,.75,.5,.5
80,.5,.75,.5
81,.5,.5,.75
156,.25,0.,.5
157,.5,0.,.5
158,.75,0.,.5
161,1.,.25,.5
162,1.,.5,.5
163,1.,.75,.5
166,.75,1.,.5
167,.5,1.,.5
168,.25,1.,.5
171,0.,.75,.5
172,0.,.5,.5
173,0.,.25,.5
176,.5,.25,.5
177,.25,.5,.5
178,.5,.5,.5
179,.75,.5,.5
180,.5,.75,.5
*ELEMENT, TYPE=C3D20R
1,1,10,47,19,37,57,78,72,9,45,
46,20,56,76,77,73,38,55,75,70
2,10,2,13,47,57,34,62,78,11,12,
48,45,58,61,79,76,55,33,60,75
3,19,47,16,4,72,78,67,43,46,49,
17,18,77,80,68,71,70,75,65,42
4,47,13,3,16,78,62,40,67,48,14,
15,49,79,63,66,80,75,60,39,65
5,37,157,178,172,5,22,52,31,156,176,
177,173,21,50,51,32,36,59,81,74
6,157,34,162,178,22,6,25,52,158,161,
179,176,23,24,53,50,59,35,64,81
7,172,178,167,43,31,52,28,8,177,180,
168,171,51,54,29,30,74,81,69,44
8,178,162,40,167,52,25,7,28,179,163,
166,180,53,26,27,54,81,64,41,69
*BOUNDARY
1,1
1,2
1,3
2,2
2,3
4,3
*EQUATION
    2
  178,    1,        1.,   78,    1,       -1.
    2
  178,    2,        1.,   78,    2,       -1.
    2
  178,    3,        1.,   78,    3,       -1.
*MATERIAL,NAME=EL
*ELASTIC
210000.,.3
*NSET,NSET=SET1,GENERATE
1,180
*ELSET,ELSET=SET2,GENERATE
1,8
*ELSET,ELSET=EALL,GENERATE
1,8
*SOLID SECTION,MATERIAL=EL,ELSET=EALL
*STEP
*STATIC
*CLOAD
5,3,1.
6,3,1.
7,3,1.
8,3,1.
*NODE PRINT,NSET=SET1
U
*EL PRINT,ELSET=SET2
S
*END STEP
```

执行 `./ccx_2.22 achtel`:

```
************************************************************

CalculiX Version 2.22, Copyright(C) 1998-2024 Guido Dhondt
CalculiX comes with ABSOLUTELY NO WARRANTY. This is free
software, and you are welcome to redistribute it under
certain conditions, see gpl.htm

************************************************************

You are using an executable made on Tue Sep  9 03:24:31 PM CST 2025

  The numbers below are estimated upper bounds

  number of:

   nodes:          180
   elements:            8
   one-dimensional elements:            0
   two-dimensional elements:            0
   integration points per element:            8
   degrees of freedom per node:            3
   layers per element:            1

   distributed facial loads:            0
   distributed volumetric loads:            0
   concentrated loads:            4
   single point constraints:            6
   multiple point constraints:            4
   terms in all multiple point constraints:            7
   tie constraints:            0
   dependent nodes tied by cyclic constraints:            0
   dependent nodes in pre-tension constraints:            0

   sets:            3
   terms in all sets:           12

   materials:            1
   constants per material and temperature:            2
   temperature points per material:            1
   plastic data points per material:            0

   orientations:            0
   amplitudes:            2
   data points in all amplitudes:            2
   print requests:            2
   transformations:            0
   property cards:            0


 STEP            1

 Static analysis was selected

 Decascading the MPC's

 Determining the structure of the matrix:
 Using up to 1 cpu(s) for setting up the structure of the matrix.
 number of equations
 285
 number of nonzero lower triangular matrix elements
 11623

 Using up to 1 cpu(s) for the stress calculation.

 Using up to 1 cpu(s) for the symmetric stiffness/mass contributions.

 Factoring the system of equations using the symmetric spooles solver
 Using 1 cpu for spooles.

 Using up to 1 cpu(s) for the stress calculation.


 Job finished

________________________________________

Total CalculiX Time: 0.181115
```

结果输出在 `achtel.dat` 中:

```
debian@revyos-lpi4a:~/CalculiX/ccx_2.22/src$ cat achtel.dat

                        S T E P       1


                                INCREMENT     1


 displacements (vx,vy,vz) for set SET1 and time  0.1000000E+01

         1  0.000000E+00  0.000000E+00  0.000000E+00
         2 -1.723861E-04  0.000000E+00  0.000000E+00
         3  2.463875E-04 -1.723861E-04  1.138229E-03
         4  4.187735E-04 -1.723861E-04  0.000000E+00
         5 -4.651418E-04 -4.651418E-04  3.591649E-04
         6 -6.375279E-04 -6.730874E-04  9.266397E-04
         7 -4.267000E-04 -8.454735E-04  1.497394E-03
         8 -2.543139E-04 -6.375279E-04  9.266397E-04
         9  8.177554E-06 -5.930963E-05  1.494931E-04
         .....
```

证明 `CalculiX` 在 RevyOS 上具有基本可用性
