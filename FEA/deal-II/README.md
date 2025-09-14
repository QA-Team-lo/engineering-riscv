### **Guide: Installing deal.II on RevyOS**

This document provides instructions for installing `XmdS` (a tool for quickly solving differential equations) on a RISC-V device running RevyOS. The `libdealii-dev` package is available in the RevyOS repository, making the installation straightforward.

#### Installation

You can install it using the `apt` package manager:

```bash
$ sudo apt install libdealii-dev
```

#### Availability Test

For the following test program:

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

Building the program results in an error:

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

**Cause**: The `dealii` package in RevyOS depends on Boost 1.83, but the package itself was built against Boost 1.74. This mismatch makes the package unusable.

#### Building from Source

* Obtain the source:

```bash
git clone https://github.com/dealii/dealii.git
```

* Build using `cmake`:

```bash
mkdir build && cd build && cmake ..
make -j4
```

**Build log excerpt:**

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
...
```

After 5 hours, the compilation still did not finish, and the development board ran out of memory.

**System log:**

```
[31088.274191] systemd-journald[261]: Under memory pressure, flushing caches.
[31281.738934] systemd-journald[261]: Under memory pressure, flushing caches.
```

**Result**: Building from source failed.



#### **RISC-V Adaptation Value**

`dealii` is still under active development (the latest version 9.7.0 was released on July 23) and has a large user base (1.5k stars on GitHub), so it is worth fixing.
