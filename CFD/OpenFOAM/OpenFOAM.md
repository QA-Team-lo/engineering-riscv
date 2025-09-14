# OpenFOAM (CFD) on RevyOS

## Prerequisites

See [Build.md](https://develop.openfoam.com/Development/openfoam/blob/develop/doc/Build.md)

## Obtain the Source Code

```
curl -L https://dl.openfoam.com/source/v2506/OpenFOAM-v2506.tgz -o OpenFOAM-v2506.tgz
tar -xvzf OpenFOAM-v2506.tgz
```

## Modify the Build Configuration for RISC-V

Remove 32-bit and i386 compile flags from `./wmake/rules/linuxIcc`, `./wmake/rules/linuxGcc`

```
grep -r -- "-m32" wmake

grep -r "elf_i386" .
```


## Compile and Install

```
foamSystemCheck

foam

./Allwmake -j -s -q -l
```

## Verification

```
debian@revyos-pioneer:~/CFD/OpenFOAM-v2506$ foamInstallationTest
Executing foamInstallationTest

Basic setup :
-------------------------------------------------------------------------------
OpenFOAM:            OpenFOAM-v2506
ThirdParty:          [missing]
This can be intentional, or indicate a faulty installation
Shell:               bash
Host:                revyos-pioneer
OS:                  Linux version 6.16.4-pioneer
-------------------------------------------------------------------------------

Main OpenFOAM env variables :
-------------------------------------------------------------------------------
Environment           FileOrDirectory                          Valid      Crit
-------------------------------------------------------------------------------
$WM_PROJECT_USER_DIR  /home/debian/OpenFOAM/debian-v2506        no        no
$WM_THIRD_PARTY_DIR   .../debian/CFD/OpenFOAM-v2506/ThirdParty  no        maybe
$WM_PROJECT_SITE      [env variable unset]                                no
-------------------------------------------------------------------------------

OpenFOAM env variables in PATH :
-------------------------------------------------------------------------------
Environment           FileOrDirectory                          Valid Path Crit
-------------------------------------------------------------------------------
$WM_PROJECT_DIR       /home/debian/CFD/OpenFOAM-v2506           yes  yes  yes

$FOAM_APPBIN          ...2506/platforms/linuxGccDPInt32Opt/bin  yes  yes  yes
$FOAM_SITE_APPBIN     ...2506/platforms/linuxGccDPInt32Opt/bin  no        no
$FOAM_USER_APPBIN     ...2506/platforms/linuxGccDPInt32Opt/bin  no        no
$WM_DIR               /home/debian/CFD/OpenFOAM-v2506/wmake     yes  yes  often
-------------------------------------------------------------------------------

OpenFOAM env variables in LD_LIBRARY_PATH :
-------------------------------------------------------------------------------
Environment           FileOrDirectory                          Valid Path Crit
-------------------------------------------------------------------------------
$FOAM_LIBBIN          ...2506/platforms/linuxGccDPInt32Opt/lib  yes  yes  yes
$FOAM_SITE_LIBBIN     ...2506/platforms/linuxGccDPInt32Opt/lib  no        no
$FOAM_USER_LIBBIN     ...2506/platforms/linuxGccDPInt32Opt/lib  no        no
$FOAM_EXT_LIBBIN      [env variable unset]                                maybe
$MPI_ARCH_PATH        /usr/lib/riscv64-linux-gnu/openmpi        yes  yes  yes
$FOAM_EXT_LIBBIN      [env variable unset]                                maybe                                        [0/1827]
$MPI_ARCH_PATH        /usr/lib/riscv64-linux-gnu/openmpi        yes  yes  yes
-------------------------------------------------------------------------------

Software Components
-------------------------------------------------------------------------------
Software     Version    Location
-------------------------------------------------------------------------------
flex         2.6.4      /usr/bin/flex
make         4.4.1      /usr/bin/make
wmake        2506       /home/debian/CFD/OpenFOAM-v2506/wmake/wmake
gcc          14.3.0     /usr/bin/gcc
g++          14.3.0     /usr/bin/g++
-------------------------------------------------------------------------------
icoFoam      exists     ...nFOAM-v2506/platforms/linuxGccDPInt32Opt/bin/icoFoam

Summary
-------------------------------------------------------------------------------
Base configuration ok.
Critical systems ok.

Done

debian@revyos-pioneer:~/CFD/OpenFOAM-v2506$ foamTestTutorial -full verificationAndValidation/schemes/divergenceExample
Run test: verificationAndValidation/schemes/divergenceExample
Restore 0/ from 0.orig/
Running blockMesh on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use limitedLinear 0.2
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use limitedLinear 1.0
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use linear
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use linearUpwind grad(U)
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use LUST grad(U)
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use midPoint
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use Minmod
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use MUSCL
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use QUICK
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use SFCD
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use SuperBee
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use UMIST
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use upwind
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
Updating fvSchemes to use vanLeer
Running scalarTransportFoam on /tmp/tmp.5NK5tJAsb1.verificationAndValidation_schemes_divergenceExample
run: OK
Passed all 1 tests 
``` 
