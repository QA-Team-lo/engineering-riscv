### **指南：在 RevyOS 上安装 FreeFEM**

本文档提供了在运行 RevyOS 的 RISC-V 设备上安装 `freefem` 的说明。`freefem++` 包在 RevyOS 仓库中可用，使安装变得非常简单。

#### 安装

可以使用 `apt` 包管理器执行安装。

```bash
$ sudo apt install freefem++
```
#### 可用性测试
```bash
debian@revyos-lpi4a:~$ FreeFem++-nw test.edp
-- FreeFem++ v4.9 ( - git no git)
 Load: lg_fem lg_mesh lg_mesh3 eigenvalue
    1 : // Define mesh boundary
    2 : border C(t=0, 2*pi){ x = cos(t); y = sin(t); }
    3 :
    4 : // The triangulated domain Th is on the left side of its boundary
    5 : mesh Th = buildmesh(C(50));
    6 :
    7 : // The finite element space defined over Th is called here Vh
    8 : fespace Vh(Th, P1);
    9 : Vh u, v; // Define u and v as piecewise-P1 continuous functions
   10 :
   11 : // Define a function f
   12 : func f = x*y;
   13 :
   14 : // Get the clock in second
   15 : real cpu = clock();
   16 :
   17 : // Define the PDE
   18 : solve Poisson(u, v, solver=LU)
   19 :     = int2d(Th)( dx(u)*dx(v) + dy(u)*dy(v) )   // bilinear part
   20 :     - int2d(Th)( f*v )                         // right hand side
   21 :     + on(C, u=0);                              // Dirichlet BC
   22 :
   23 : // Plot the result
   24 : plot(u);
   25 :
   26 : // Display the total computational time
   27 : cout << "CPU time = " << (clock()-cpu) << endl; sizestack + 1024 =1920  ( 896 )


  --  mesh:  Nb of Triangles =    434, Nb of Vertices 243
  SkyLineMatrix: size pL/pU: 243 3732 3732 moy=15.358
  -- Solve :
          min -0.010328  max 0.0102877
CPU time = 0.043329
times: compile 0.103533s, execution 0.11013s,  mpirank:0
 CodeAlloc : nb ptr  3646,  size :489680 mpirank: 0
Ok: Normal End
```

证明 `FreeFem` 在 RevyOS 上初步可用。

#### 其它情况

在不支持 OpenGL GLX 拓展的环境中，FreeFem 的图形界面无法正常使用，例如:

```
debian@revyos-lpi4a:~$ FreeFem++ test.edp
-- FreeFem++ v4.9 ( - git no git)
 Load: lg_fem lg_mesh lg_mesh3 eigenvalue
    1 : // Define mesh boundary
    2 : border C(t=0, 2*pi){ x = cos(t); y = sin(t); }
    3 :
    4 : // The triangulated domain Th is on the left side of its boundary
    5 : mesh Th = buildmesh(C(50));
    6 :
    7 : // The finite element space defined over Th is called here Vh
    8 : fespace Vh(Th, P1);
    9 : Vh u, v; // Define u and v as piecewise-P1 continuous functions
   10 :
   11 : // Define a function f
   12 : func f = x*y;
   13 :
   14 : // Get the clock in second
   15 : real cpu = clock();
   16 :
   17 : // Define the PDE
   18 : solve Poisson(u, v, solver=LU)
   19 :     = int2d(Th)( dx(u)*dx(v) + dy(u)*dy(v) )   // bilinear part
   20 :     - int2d(Th)( f*v )                         // right hand side
   21 :     + on(C, u=0);                              // Dirichlet BC
   22 :
   23 : // Plot the result
   24 : plot(u);
   25 :
   26 : // Display the total computational time
   27 : cout << "CPU time = " << (clock()-cpu) << endl; sizestack + 1024 =1920  ( 896 )

  --  mesh:  Nb of Triangles =    434, Nb of Vertices 243
  SkyLineMatrix: size pL/pU: 243 3732 3732 moy=15.358
  -- Solve :
          min -0.010328  max 0.0102877
CPU time = 0.01721
times: compile 0.052268s, execution 0.036198s,  mpirank:0
freeglut (ffglut): OpenGL GLX extension not supported by display ':0.0'
 CodeAlloc : nb ptr  3646,  size :489680 mpirank: 0
Ok: Normal End
Segmentation fault
```
