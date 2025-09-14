# libMesh (CFD) on RevyOS

## Obtain the Source Code

```
wget https://github.com/libMesh/libmesh/releases/download/v1.8.1/libmesh-1.8.1.tar.gz
tar -xvzf libmesh-1.8.1.tar.gz
mkdir build
cd build
../configure --prefix=/home/debian/CFD/libmesh/
```

## Compile and Install

```
make 
make check 
```

Can't pass libMesh's example programs and unit tests.

```
# make check log above

--------------------------------------------------------------------------
MPI_ABORT was invoked on rank 0 in communicator MPI_COMM_WORLD
  Proc: [[28941,0],0]
  Errorcode: 1

NOTE: invoking MPI_ABORT causes Open MPI to kill all MPI processes.
You may or may not see output from other processes, depending on
exactly when Open MPI kills them.
--------------------------------------------------------------------------
FAIL: run.sh
===============================================================
1 of 1 test failed
Please report to https://github.com/libMesh/libmesh/discussions
===============================================================
make[3]: *** [Makefile:1004: check-TESTS] Error 1
make[3]: Leaving directory '/home/debian/CFD/libmesh-1.8.1/build/examples/systems_of_equations/systems_of_equations_ex3'
make[2]: *** [Makefile:1130: check-am] Error 2
make[2]: Leaving directory '/home/debian/CFD/libmesh-1.8.1/build/examples/systems_of_equations/systems_of_equations_ex3'
make[1]: *** [Makefile:757: check-recursive] Error 1
make[1]: Leaving directory '/home/debian/CFD/libmesh-1.8.1/build/examples'
make: *** [Makefile:34348: check-recursive] Error 1
```

