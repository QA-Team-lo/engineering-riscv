### 在 RevyOS 上安装 Varkon

目前需要从源码编译，且需要对 Makefile 进行一些修改，但仍编译失败。

#### 编译 Varkon

依赖包安装：

```shell
sudo apt update
sudo apt install -y libtirpc-dev libxpm-dev
```

需要用此目录下提供的 [Makefile.linux](files/Makefile.linux) 替换 `EX/src/Makefile.linux`。

运行：

```shell
make
```

遇到如下问题：

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

构建未能完成。