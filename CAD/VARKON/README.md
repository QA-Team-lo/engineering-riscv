### Install Varkon on RevyOS

Currently, building from source is required, plus some modifications to Makefile. But still FTBFS.

#### BUilding Varkon

Install dependencies:

```shell
sudo apt update
sudo apt install -y libtirpc-dev libxpm-dev
```

Use [Makefile.linux](files/Makefile.linux) here to replace `EX/src/Makefile.linux`。

Then run:

```shell
make
```

Build failed with the following:

```log
make[1]: Entering directory '/home/debian/CAD/Varkon_1.19E/sources'
cc IG/lib/IGlib.a PM/lib/PMlib.a EX/lib/EXlib.a DB/lib/DBlib.a WP/lib/WPlib.a GE/lib/GElib.a  -L/usr/X11R6/lib -lX11 -lXext -lXpm -lGL -lGLU -ltiff  -lm -o ../bin/xvarkon
/usr/bin/ld: IG/lib/IGlib.a(iginit.o): relocations in generic ELF (EM: 3)
/usr/bin/ld: IG/lib/IGlib.a(iginit.o): relocations in generic ELF (EM: 3)
/usr/bin/ld: IG/lib/IGlib.a(iginit.o): relocations in generic ELF (EM: 3)
/usr/bin/ld: IG/lib/IGlib.a: error adding symbols: file in wrong format
collect2: error: ld returned 1 exit status
make[1]: *** [Makefile.linux:58: ../bin/xvarkon] Error 1
make[1]: Leaving directory '/home/debian/CAD/Varkon_1.19E/sources'
make: *** [Makefile:75: distr] Error 2
```