# SLFCFD (CFD) on RevyOS

## Prerequisites

- FFmpeg
- Mesa 3.0 or OpenGL
- gcc
- perl

## Obtain the Source Code

```
wget https://slfcfd.sourceforge.net/slfcfd12.tgz
tar xzvfp slfcfd12.tgz
```

## Compile and Install

Can't compile use modern gcc

```
debian@revyos-pioneer:~/CFD/slfcfd-1.2$ make CFLAGS="-g -std=c89" everything
cd common ;  make
make[1]: Entering directory '/home/debian/CFD/slfcfd-1.2/common'
/usr/bin/gcc  -g -c crossx.c
/usr/bin/gcc  -g -c dotx.c
/usr/bin/gcc  -g -c matx.c
/usr/bin/gcc  -g -c matxt.c
/usr/bin/gcc  -g -c mat3x3.c
/usr/bin/gcc  -g -c matsquare.c
/usr/bin/gcc  -g -c lusymdcm.c
/usr/bin/gcc  -g -c lusymbks.c
/usr/bin/gcc  -g -c conj_pois.c
/usr/bin/gcc  -g -c line_count.c
/usr/bin/gcc  -g -c flowlines_file.c
flowlines_file.c: In function 'Flowlines_File':
flowlines_file.c:47:37: error: implicit declaration of function 'line_count' [-Wimplicit-function-declaration]
   47 |                 line_count_number = line_count( flow[0].file_in );
      |                                     ^~~~~~~~~~
make[1]: *** [Makefile:65: flowlines_file.o] Error 1
make[1]: Leaving directory '/home/debian/CFD/slfcfd-1.2/common'
make: *** [Makefile:37: everything] Error 2
```
