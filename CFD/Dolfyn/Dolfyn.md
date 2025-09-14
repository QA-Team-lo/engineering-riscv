# Dolfyn (CFD) on RevyOS

## Prerequisites and Obtain the Source Code

Because can't find source code from [dolfyn.net](https://www.dolfyn.net), we use [PhoronixTestSuite](https://openbenchmarking.org/test/pts/dolfyn)'s version

```
wget http://www.phoronix-test-suite.com/benchmark-files/dolfyn-cfd_0.527.tgz
tar -xzvf dolfyn-cfd_0.527.tgz
cd dolfyn-cfd_0.527/src/
```

## Modify the Code and Build Configuration

```
sed -i "s/     stop'bug: error in dimensions of array v' /     stop 'bug: error in dimensions of array v' /g" gmsh2dolfyn.f90
```

## Compile and Install

```
make
```

## Verification

```
cd ../demo
./doit.sh 
```

Full benchmark log can see [dolfyn.log](./dolfyn.log)

```
 *** CONVERGENCE ***   8.92726239E-05
 Opening restart file
 Data
 U
 V
 W
 P
 KE
 ED
 T
 Density
 VisEff
 Cp
 Mass flux
 Boundaries
 Restartfile 400 written
 Writing VTK data file...
 Maximum change in PP:   1.25094000E-02   8.92726239E-05   9.99999975E-05
 Arrays cleaned up
 Watches     :           15 >           6
 Elapsed time:    8.36350024E-02
  W L RA         :                             cpu       %
  1 1 FF *       : Master                   3.096E-02  37.02
 10 2 FF **      : CalculatePressure        1.068E-02  12.77
  8 2 FF **      : CalculateUVW             1.034E-02  12.37
  9 3 FF ***     : FluxUVW                  5.792E-03   6.93
  4 2 FF **      : Venkatarishnan           3.922E-03   4.69
  2 2 FF **      : ReadGeometry             3.876E-03   4.63
 11 3 FF ***     : FluxMass                 3.727E-03   4.46
 13 3 FF ***     : Venkatarishnan           2.969E-03   3.55
 12 3 FF ***     : GaussPressCor            1.873E-03   2.24
  3 2 FF **      : GaussU                   1.188E-03   1.42
  7 2 FF **      : GaussPressure            9.740E-04   1.16
  5 2 FF **      : GaussV                   9.450E-04   1.13
  6 2 FF **      : GaussW                   9.420E-04   1.13
 15 3 FF ***     : GaussPressure            9.070E-04   1.08
 14 3 FF ***     : FluxMass2                5.300E-04   0.63
                                            ---------  -----
                                            7.963E-02  95.21
 Done wes2
```

