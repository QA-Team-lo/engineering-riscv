# Gerris (CFD) on RevyOS

## Obtain the Source Code

```
curl -L https://sourceforge.net/projects/gfs/files/gerris/0.8.0/gerris-0.8.0.tar.gz/download -o gerris-0.8.0.tar.gz

tar -xvzf gerris-0.8.0.tar.gz
```

## Modify the Build Configuration

Get new `config.guess`

```
wget -O config.guess 'https://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD'
wget -O config.sub 'https://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub;hb=HEAD'
```

## Compile and Install

Code not support 64 bit architecture

```log
debian@revyos-pioneer ~/C/gerris-0.8.0> ./configure
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking whether to enable maintainer-specific portions of Makefiles... no
checking for mpicc... yes
checking for gcc... mpicc
checking for C compiler default output file name... a.out
checking whether the C compiler works... yes
checking whether we are cross compiling... no
checking for suffix of executables...
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether mpicc accepts -g... yes
checking for mpicc option to accept ANSI C... none needed
checking for style of include used by make... GNU
checking dependency style of mpicc... gcc3
checking build system type... riscv64-unknown-linux-gnu
checking host system type... riscv64-unknown-linux-gnu
checking for a sed that does not truncate output... /usr/bin/sed
checking for egrep... grep -E
checking for ld used by mpicc... /usr/bin/ld
checking if the linker (/usr/bin/ld) is GNU ld... yes
checking for /usr/bin/ld option to reload object files... -r
checking for BSD-compatible nm... /usr/bin/nm -B
checking whether ln -s works... yes
checking how to recognise dependent libraries... pass_all
checking how to run the C preprocessor... mpicc -E
checking for ANSI C header files... no
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking dlfcn.h usability... yes
checking dlfcn.h presence... yes
checking for dlfcn.h... yes
checking for g++... g++
checking whether we are using the GNU C++ compiler... yes
checking whether g++ accepts -g... yes
checking dependency style of g++... gcc3
checking how to run the C++ preprocessor... g++ -E
checking for g77... no
checking for f77... f77
checking whether we are using the GNU Fortran 77 compiler... yes
checking whether f77 accepts -g... yes
checking the maximum length of command line arguments... 32768
checking command to parse /usr/bin/nm -B output from mpicc object... ok
checking for objdir... .libs
checking for ar... ar
checking for ranlib... ranlib
checking for strip... strip
checking if mpicc static flag  works... yes
checking if mpicc supports -fno-rtti -fno-exceptions... no
checking for mpicc option to produce PIC... -fPIC
checking if mpicc PIC flag -fPIC works... yes
checking if mpicc supports -c -o file.o... yes
checking whether the mpicc linker (/usr/bin/ld) supports shared libraries... yes
checking whether -lc should be explicitly linked in... no
checking dynamic linker characteristics... GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
checking whether stripping libraries is possible... yes
checking if libtool supports shared libraries... yes
checking whether to build shared libraries... yes
checking whether to build static libraries... yes
configure: creating libtool
appending configuration tag "CXX" to libtool
checking for ld used by g++... /usr/bin/ld
checking if the linker (/usr/bin/ld) is GNU ld... yes
checking whether the g++ linker (/usr/bin/ld) supports shared libraries... yes
checking for g++ option to produce PIC... -fPIC
checking if g++ PIC flag -fPIC works... yes
checking if g++ supports -c -o file.o... yes
checking whether the g++ linker (/usr/bin/ld) supports shared libraries... yes
checking dynamic linker characteristics... GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
checking whether stripping libraries is possible... yes
appending configuration tag "F77" to libtool
checking if libtool supports shared libraries... yes
checking whether to build shared libraries... yes
checking whether to build static libraries... yes
checking for f77 option to produce PIC... -fPIC
checking if f77 PIC flag -fPIC works... yes
checking if f77 supports -c -o file.o... yes
checking whether the f77 linker (/usr/bin/ld) supports shared libraries... yes
checking dynamic linker characteristics... GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
checking whether stripping libraries is possible... yes
checking for gawk... (cached) gawk
checking for strerror in -lcposix... no
checking for ANSI C header files... (cached) no
checking whether pointers can be stored in doubles... no
configure: error:
*** Pointers cannot be stored in doubles on this architecture.
```

