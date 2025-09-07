# OpenFlower (CFD) on RevyOS

## Prerequisites

## Obtain the Source Code

```
curl -L "https://sourceforge.net/projects/openflower/files/OpenFlower/v1.0/openflower-v1.0.tar.gz/download" -o openflower-v1.0.tar.gz
tar -zxvf openflower-v1.0.tar.gz
``` 

## Modify the Build Configuration for RISC-V

```
CPPFLAGS="-I$HOME/include" LDFLAGS="-L$HOME/lib" ./configure
```

## Compile and Install

can't find needed c library 

```
debian@revyos-pioneer ~/C/openflower-v1.0> make
make  all-recursive
make[1]: Entering directory '/home/debian/CFD/openflower-v1.0'
Making all in src
make[2]: Entering directory '/home/debian/CFD/openflower-v1.0/src'
Making all in Algorithms
make[3]: Entering directory '/home/debian/CFD/openflower-v1.0/src/Algorithms'
g++ -DHAVE_CONFIG_H -I. -I../..   -I/home/debian/include  -g -O2 -MT Algorithm.o -MD -MP -MF .deps/Algorithm.Tpo -c -o Algorithm.o Algorithm.cpp
In file included from Algorithm.cpp:8:
Algorithm.h: In member function ‘virtual double Algorithm::getStabilityTimeStep()’:
Algorithm.h:27:41: warning: no return statement in function returning non-void [-Wreturn-type]
   27 |   virtual double getStabilityTimeStep(){} //! Calculates the Stability Time Step specific to this Algorithm.
      |                                         ^
mv -f .deps/Algorithm.Tpo .deps/Algorithm.Po
g++ -DHAVE_CONFIG_H -I. -I../..   -I/home/debian/include  -g -O2 -MT AlgoAllenCahn.o -MD -MP -MF .deps/AlgoAllenCahn.Tpo -c -o AlgoAllenCahn.o AlgoAllenCahn.cpp
In file included from AlgoAllenCahn.h:8,
                 from AlgoAllenCahn.cpp:7:
Algorithm.h: In member function ‘virtual double Algorithm::getStabilityTimeStep()’:
Algorithm.h:27:41: warning: no return statement in function returning non-void [-Wreturn-type]
   27 |   virtual double getStabilityTimeStep(){} //! Calculates the Stability Time Step specific to this Algorithm.
      |                                         ^
mv -f .deps/AlgoAllenCahn.Tpo .deps/AlgoAllenCahn.Po
g++ -DHAVE_CONFIG_H -I. -I../..   -I/home/debian/include  -g -O2 -MT AlgoScalarDiff.o -MD -MP -MF .deps/AlgoScalarDiff.Tpo -c -o AlgoScalarDiff.o AlgoScalarDiff.cpp
In file included from AlgoScalarDiff.h:8,
                 from AlgoScalarDiff.cpp:8:
Algorithm.h: In member function ‘virtual double Algorithm::getStabilityTimeStep()’:
Algorithm.h:27:41: warning: no return statement in function returning non-void [-Wreturn-type]
   27 |   virtual double getStabilityTimeStep(){} //! Calculates the Stability Time Step specific to this Algorithm.
      |                                         ^
mv -f .deps/AlgoScalarDiff.Tpo .deps/AlgoScalarDiff.Po
g++ -DHAVE_CONFIG_H -I. -I../..   -I/home/debian/include  -g -O2 -MT AlgoNavierStokes.o -MD -MP -MF .deps/AlgoNavierStokes.Tpo -c -o AlgoNavierStokes.o AlgoNavierStokes.cpp
In file included from AlgoNavierStokes.cpp:18:
../Maths/LinAlg/Solver.h:24:10: fatal error: xc/getopts.h: No such file or directory
   24 | #include "xc/getopts.h"
      |          ^~~~~~~~~~~~~~
compilation terminated.
make[3]: *** [Makefile:339: AlgoNavierStokes.o] Error 1
make[3]: Leaving directory '/home/debian/CFD/openflower-v1.0/src/Algorithms'
make[2]: *** [Makefile:448: all-recursive] Error 1
make[2]: Leaving directory '/home/debian/CFD/openflower-v1.0/src'
make[1]: *** [Makefile:367: all-recursive] Error 1
make[1]: Leaving directory '/home/debian/CFD/openflower-v1.0'
make: *** [Makefile:308: all] Error 2
```

## Verification
