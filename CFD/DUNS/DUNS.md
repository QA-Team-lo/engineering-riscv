# DUNS (CFD) on RevyOS

## Obtain the Source Code

```
curl -L "https://sourceforge.net/projects/duns/files/duns/duns-2.7.1/duns-2.7.1-src.tar.bz2/download" -o duns-2.7.1-src.tar.bz2
tar -xvjf duns-2.7.1-src.tar.bz2
```

## Modify the Build Configuration for RISC-V

Modify `makesystem/Makesystem_linux` use `gfortran` instead `F77     = g77`

## Compile and Install

Compile FORTRAN libraries used by the DUNS code first.

```
DUNSARCH=linux; export DUNSARCH
DUNSPATH27=/home/debian/CFD/duns-2.7.1; export DUNSPATH27
cd lib/
make
```

Can't compile FORTRAN libraries

```
debian@revyos-pioneer:~/CFD/duns-2.7.1/lib$ make
gfortran -O2 -w  -c acklib.f
gfortran -O2 -w  -c mcklib.f
gfortran -O2 -w  -c tranlib.f
gfortran -O2 -w  -c cklib.f
ar vr libck.a acklib.o mcklib.o tranlib.o cklib.o
ar: creating libck.a
a - acklib.o
a - mcklib.o
a - tranlib.o
a - cklib.o
gfortran -O2 -w  -c math.f
math.f:1998:41:

 1998 |         CALL DP1VLU (NDEG,NDER,X(I),R(I),YP,A)
      |                                         1
Error: Rank mismatch in argument 'yp' at (1) (rank-1 and scalar)
math.f:2350:41:

 2350 |         CALL PVALUE (NDEG,NDER,X(I),R(I),YP,A)
      |                                         1
Error: Rank mismatch in argument 'yp' at (1) (rank-1 and scalar)
make: *** [/home/debian/CFD/duns-2.7.1/makesystem/Makesystem_linux:61: math.o] Error 1
```
