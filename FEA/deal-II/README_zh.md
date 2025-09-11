### **指南：在 RevyOS 上安装 deal-II**

本文档提供了在运行 RevyOS 的 RISC-V 设备上安装 `XmdS`（用于快速求解微分方程的工具）的说明。`libdealii-dev` 包在 RevyOS 仓库中可用，使安装变得非常简单。

#### 安装

可以使用 `apt` 包管理器执行安装。

```bash
$ sudo apt install libdealii-dev
```

#### 可用性测试

对于以下的测试程序

```c++
/* ------------------------------------------------------------------------
 *
 * SPDX-License-Identifier: LGPL-2.1-or-later
 * Copyright (C) 1999 - 2023 by the deal.II authors
 *
 * This file is part of the deal.II library.
 *
 * Part of the source code is dual licensed under Apache-2.0 WITH
 * LLVM-exception OR LGPL-2.1-or-later. Detailed license information
 * governing the source code and code contributions can be found in
 * LICENSE.md and CONTRIBUTING.md at the top level directory of deal.II.
 *
 * ------------------------------------------------------------------------
 */


#include <deal.II/grid/tria.h>
#include <deal.II/grid/grid_generator.h>
#include <deal.II/grid/grid_out.h>

#include <iostream>
#include <fstream>
#include <cmath>

using namespace dealii;

void first_grid()
{
  Triangulation<2> triangulation;

  GridGenerator::hyper_cube(triangulation);
  triangulation.refine_global(4);

  std::ofstream out("grid-1.svg");
  GridOut       grid_out;
  grid_out.write_svg(triangulation, out);
  std::cout << "Grid written to grid-1.svg" << std::endl;
}

void second_grid()
{
  Triangulation<2> triangulation;

  const Point<2> center(1, 0);
  const double   inner_radius = 0.5, outer_radius = 1.0;
  GridGenerator::hyper_shell(
    triangulation, center, inner_radius, outer_radius, 10);
  for (unsigned int step = 0; step < 5; ++step)
    {
      for (const auto &cell : triangulation.active_cell_iterators())
        {
          for (const auto v : cell->vertex_indices())
            {
              const double distance_from_center =
                center.distance(cell->vertex(v));

              if (std::fabs(distance_from_center - inner_radius) <=
                  1e-6 * inner_radius)
                {
                  cell->set_refine_flag();
                  break;
                }
            }
        }
      triangulation.execute_coarsening_and_refinement();
    }

  std::ofstream out("grid-2.svg");
  GridOut       grid_out;
  grid_out.write_svg(triangulation, out);

  std::cout << "Grid written to grid-2.svg" << std::endl;
}

int main()
{
  first_grid();
  second_grid();
}
```

构建时会报错:

```bash
debian@revyos-lpi4a:~/dealii/build$ make
[ 50%] Building CXX object CMakeFiles/foo.dir/foo.cc.o
In file included from /usr/include/deal.II/grid/tria.h:20,
                 from /home/debian/dealii/foo.cc:17:
/usr/include/deal.II/base/config.h:555:17: error: static assertion failed: The version number of boost that you are compiling with does not match the version number of boost found during deal.II's configuration step. This leads to difficult to understand bugs and is not supported. Please check that you have set up your application with the same version of boost as deal.II.


  555 |   BOOST_VERSION == 100000 * DEAL_II_BOOST_VERSION_MAJOR +
      |                 ^
/usr/include/deal.II/base/config.h:555:17: note: the comparison reduces to ‘(108300 == 107400)’

make[2]: *** [CMakeFiles/foo.dir/build.make:76: CMakeFiles/foo.dir/foo.cc.o] Error 1
make[1]: *** [CMakeFiles/Makefile2:83: CMakeFiles/foo.dir/all] Error 2
make: *** [Makefile:91: all] Error 2
```

原因: RevyOS 中的 `dealii` 软件包依赖 1.83 版本的 boost 库，但软件包本身是由 1.74 版本的 boost 库构建的，导致其无法正常使用。

#### 从源码构建

- 获取源码
```bash
git clone https://github.com/dealii/dealii.git
```

- 使用 `cmake` 构建

```bash
mkdir build && cd build && cmake ..
```

构建日志:
```bash
[ 53%] Built target object_matrix_free_release
[ 53%] Building CXX object source/fe/CMakeFiles/object_fe_release.dir/fe_tools.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_integrated_legendre_sz.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomial_space.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_p.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_piecewise.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_pyramid.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_rannacher_turek.cc.o
[ 53%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_raviart_thomas.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_rt_bubbles.cc.o
[ 54%] Building CXX object source/fe/CMakeFiles/object_fe_release.dir/fe_tools_interpolate.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_vector_anisotropic.cc.o
[ 54%] Building CXX object source/numerics/CMakeFiles/object_numerics_release.dir/vector_tools_project_hp.cc.o
[ 54%] Building CXX object source/numerics/CMakeFiles/object_numerics_release.dir/vector_tools_project_codim.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/polynomials_wedge.cc.o
[ 54%] Building CXX object source/fe/CMakeFiles/object_fe_release.dir/fe_tools_extrapolate.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/qprojector.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/quadrature.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/quadrature_lib.cc.o
[ 54%] Building CXX object source/numerics/CMakeFiles/object_numerics_release.dir/vector_tools_project_qp.cc.o
[ 54%] Building CXX object source/fe/CMakeFiles/object_fe_release.dir/mapping_q1_eulerian.cc.o
[ 54%] Building CXX object source/cgal/CMakeFiles/object_cgal_release.dir/surface_mesh.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/quadrature_selector.cc.o
[ 54%] Building CXX object source/fe/CMakeFiles/object_fe_release.dir/mapping_q_eulerian.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/scalar_polynomials_base.cc.o
[ 54%] Building CXX object source/cgal/CMakeFiles/object_cgal_release.dir/intersections.cc.o
[ 54%] Building CXX object source/base/CMakeFiles/object_base_release.dir/symbolic_function.cc.o
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/table_handler.cc.o
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/tensor.cc.o
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/tensor_function.cc.o
[ 55%] Built target object_fe_release
[ 55%] Building CXX object source/gmsh/CMakeFiles/object_gmsh_release.dir/utilities.cc.o
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/tensor_function_parser.cc.o
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/tensor_product_polynomials.cc.o
[ 55%] Built target object_gmsh_release
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/tensor_product_polynomials_bubbles.cc.o
[ 55%] Building CXX object source/base/CMakeFiles/object_base_release.dir/tensor_product_polynomials_const.cc.o
[ 56%] Building CXX object source/grid/CMakeFiles/object_grid_release.dir/cell_id.cc.o
[ 56%] Building CXX object source/base/CMakeFiles/object_base_release.dir/thread_management.cc.o
[ 56%] Building CXX object source/base/CMakeFiles/object_base_release.dir/timer.cc.o
[ 56%] Building CXX object source/grid/CMakeFiles/object_grid_release.dir/grid_refinement.cc.o
[ 56%] Building CXX object source/numerics/CMakeFiles/object_numerics_release.dir/vector_tools_project_qpmf.cc.o
[ 56%] Building CXX object source/base/CMakeFiles/object_base_release.dir/time_stepping.cc.o
[ 56%] Building CXX object source/grid/CMakeFiles/object_grid_release.dir/intergrid_map.cc.o
[ 57%] Building CXX object source/base/CMakeFiles/object_base_release.dir/trilinos_utilities.cc.o
[ 57%] Building CXX object source/base/CMakeFiles/object_base_release.dir/utilities.cc.o
[ 57%] Building CXX object source/grid/CMakeFiles/object_grid_release.dir/manifold.cc.o
[ 57%] Building CXX object source/base/CMakeFiles/object_base_release.dir/vectorization.cc.o
[ 57%] Building CXX object source/base/CMakeFiles/object_base_release.dir/function_cspline.cc.o
[ 57%] Building CXX object source/base/CMakeFiles/object_base_release.dir/data_out_base.cc.o
[ 57%] Building CXX object source/grid/CMakeFiles/object_grid_release.dir/manifold_lib.cc.o
```

十小时后，编译仍然没有完成，同时开发板内存耗尽。

系统日志:
```
[31088.274191] systemd-journald[261]: Under memory pressure, flushing caches.
[31281.738934] systemd-journald[261]: Under memory pressure, flushing caches.
```

结果: 从源码构建失败
