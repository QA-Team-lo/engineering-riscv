# 指南：在 RevyOS 上构建 Tessa

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 Tessa 软件的说明。

该软件包似乎很久以前就已从 Debian 中删除 (https://tracker.debian.org/pkg/tessa)，至今仍未找到任何已知的 VCS 链接（除了其原始论文 https://www.researchgate.net/publication/3121201_Three-Dimensional_optical_pulse_simulation_using_the_FDTD_method）。

不过，其源代码副本仍可在以下位置获取：https://snapshot.debian.org/archive/debian/20050312T000000Z/pool/main/t/tessa/tessa_0.3.1.orig.tar.gz

## 第一步: 解压源码并编译

```bash
tar xvf tessa_0.3.1.orig.tar.gz
cd tessa-0.3.1
CFLAGS="`pkg-config --cflags hdf5`" LDFLAGS="`pkg-config --libs-only-L hdf5`" ./configure # see https://github.com/Unidata/netcdf-c/issues/1216
make
```

## 结果

由于以下错误，构建失败：

```log
debian@revyos-pioneer:~/tessa-0.3.1$ make -j64
Making all in src
make[1]: 进入目录“/home/debian/tessa-0.3.1/src”
make  all-am
make[2]: 进入目录“/home/debian/tessa-0.3.1/src”
if gcc -DHAVE_CONFIG_H -I. -I. -I.    -Wall -I/usr/include/hdf5/serial -MT arrayh5.o -MD -MP -MF ".deps/arrayh5.Tpo" \
  -c -o arrayh5.o `test -f 'arrayh5.c' || echo './'`arrayh5.c; \
then mv -f ".deps/arrayh5.Tpo" ".deps/arrayh5.Po"; \
else rm -f ".deps/arrayh5.Tpo"; exit 1; \
fi
if gcc -DHAVE_CONFIG_H -I. -I. -I.    -Wall -I/usr/include/hdf5/serial -MT init.o -MD -MP -MF ".deps/init.Tpo" \
  -c -o init.o `test -f 'init.c' || echo './'`init.c; \
then mv -f ".deps/init.Tpo" ".deps/init.Po"; \
else rm -f ".deps/init.Tpo"; exit 1; \
fi
In file included from /usr/include/hdf5/serial/H5public.h:31,
                 from /usr/include/hdf5/serial/hdf5.h:21,
                 from arrayh5.c:26:
arrayh5.c: In function ‘arrayh5_read’:
/usr/include/hdf5/serial/H5version.h:727:19: error: too few arguments to function ‘H5Dopen2’
  727 |   #define H5Dopen H5Dopen2
      |                   ^~~~~~~~
arrayh5.c:216:16: note: in expansion of macro ‘H5Dopen’
  216 |      data_id = H5Dopen(file_id, dname);
      |                ^~~~~~~
In file included from /usr/include/hdf5/serial/hdf5.h:24:
/usr/include/hdf5/serial/H5Dpublic.h:406:14: note: declared here
  406 | H5_DLL hid_t H5Dopen2(hid_t loc_id, const char *name, hid_t dapl_id);
      |              ^~~~~~~~
arrayh5.c:272:36: warning: pointer targets in passing argument 3 of ‘H5Sselect_hyperslab’ differ in signedness [-Wpointer-sign]
  272 |                                    start, NULL, count, NULL);
      |                                    ^~~~~
      |                                    |
      |                                    hssize_t * {aka long int *}
In file included from /usr/include/hdf5/serial/H5Ppublic.h:29,
                 from /usr/include/hdf5/serial/hdf5.h:35:
/usr/include/hdf5/serial/H5Spublic.h:1215:83: note: expected ‘const hsize_t *’ {aka ‘const long unsigned int *’} but argument is of type ‘hssize_t *’ {aka ‘long int *’}
 1215 | H5_DLL herr_t H5Sselect_hyperslab(hid_t space_id, H5S_seloper_t op, const hsize_t start[],
      |                                                                     ~~~~~~~~~~~~~~^~~~~~~
arrayh5.c:289:36: warning: pointer targets in passing argument 3 of ‘H5Sselect_hyperslab’ differ in signedness [-Wpointer-sign]
  289 |                                    start, NULL, count2, NULL);
      |                                    ^~~~~
      |                                    |
      |                                    hssize_t * {aka long int *}
/usr/include/hdf5/serial/H5Spublic.h:1215:83: note: expected ‘const hsize_t *’ {aka ‘const long unsigned int *’} but argument is of type ‘hssize_t *’ {aka ‘long int *’}
 1215 | H5_DLL herr_t H5Sselect_hyperslab(hid_t space_id, H5S_seloper_t op, const hsize_t start[],
      |                                                                     ~~~~~~~~~~~~~~^~~~~~~
arrayh5.c: In function ‘dataset_exists’:
arrayh5.c:41:18: error: passing argument 1 of ‘H5Eget_auto2’ makes integer from pointer without a cast [-Wint-conversion]
   41 |      H5Eget_auto(&xxxxx_err_func, &xxxxx_err_func_data); \
      |                  ^~~~~~~~~~~~~~~
      |                  |
      |                  herr_t (**)(hid_t,  void *) {aka int (**)(long int,  void *)}
arrayh5.c:332:6: note: in expansion of macro ‘SUPPRESS_HDF5_ERRORS’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
In file included from /usr/include/hdf5/serial/hdf5.h:25:
/usr/include/hdf5/serial/H5Epublic.h:613:34: note: expected ‘hid_t’ {aka ‘long int’} but argument is of type ‘herr_t (**)(hid_t,  void *)’ {aka ‘int (**)(long int,  void *)’}
  613 | H5_DLL herr_t H5Eget_auto2(hid_t estack_id, H5E_auto2_t *func, void **client_data);
      |                            ~~~~~~^~~~~~~~~
arrayh5.c:41:35: error: passing argument 2 of ‘H5Eget_auto2’ from incompatible pointer type [-Wincompatible-pointer-types]
   41 |      H5Eget_auto(&xxxxx_err_func, &xxxxx_err_func_data); \
      |                                   ^~~~~~~~~~~~~~~~~~~~
      |                                   |
      |                                   void **
arrayh5.c:332:6: note: in expansion of macro ‘SUPPRESS_HDF5_ERRORS’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:613:58: note: expected ‘herr_t (**)(hid_t,  void *)’ {aka ‘int (**)(long int,  void *)’} but argument is of type ‘void **’
  613 | H5_DLL herr_t H5Eget_auto2(hid_t estack_id, H5E_auto2_t *func, void **client_data);
      |                                             ~~~~~~~~~~~~~^~~~
/usr/include/hdf5/serial/H5version.h:749:23: error: too few arguments to function ‘H5Eget_auto2’
  749 |   #define H5Eget_auto H5Eget_auto2
      |                       ^~~~~~~~~~~~
arrayh5.c:41:6: note: in expansion of macro ‘H5Eget_auto’
   41 |      H5Eget_auto(&xxxxx_err_func, &xxxxx_err_func_data); \
      |      ^~~~~~~~~~~
arrayh5.c:332:6: note: in expansion of macro ‘SUPPRESS_HDF5_ERRORS’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:613:15: note: declared here
  613 | H5_DLL herr_t H5Eget_auto2(hid_t estack_id, H5E_auto2_t *func, void **client_data);
      |               ^~~~~~~~~~~~
arrayh5.c:332:6: error: passing argument 1 of ‘H5Eset_auto2’ makes integer from pointer without a cast [-Wint-conversion]
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
      |      |
      |      void *
/usr/include/hdf5/serial/H5Epublic.h:649:34: note: expected ‘hid_t’ {aka ‘long int’} but argument is of type ‘void *’
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |                            ~~~~~~^~~~~~~~~
/usr/include/hdf5/serial/H5version.h:782:23: error: too few arguments to function ‘H5Eset_auto2’
  782 |   #define H5Eset_auto H5Eset_auto2
      |                       ^~~~~~~~~~~~
arrayh5.c:42:6: note: in expansion of macro ‘H5Eset_auto’
   42 |      H5Eset_auto(NULL, NULL); \
      |      ^~~~~~~~~~~
arrayh5.c:332:6: note: in expansion of macro ‘SUPPRESS_HDF5_ERRORS’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:649:15: note: declared here
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |               ^~~~~~~~~~~~
/usr/include/hdf5/serial/H5version.h:727:19: error: too few arguments to function ‘H5Dopen2’
  727 |   #define H5Dopen H5Dopen2
      |                   ^~~~~~~~
arrayh5.c:43:8: note: in definition of macro ‘SUPPRESS_HDF5_ERRORS’
   43 |      { statements; } \
      |        ^~~~~~~~~~
arrayh5.c:332:37: note: in expansion of macro ‘H5Dopen’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |                                     ^~~~~~~
/usr/include/hdf5/serial/H5Dpublic.h:406:14: note: declared here
  406 | H5_DLL hid_t H5Dopen2(hid_t loc_id, const char *name, hid_t dapl_id);
      |              ^~~~~~~~
arrayh5.c:44:18: error: passing argument 1 of ‘H5Eset_auto2’ makes integer from pointer without a cast [-Wint-conversion]
   44 |      H5Eset_auto(xxxxx_err_func, xxxxx_err_func_data); \
      |                  ^~~~~~~~~~~~~~
      |                  |
      |                  H5E_auto2_t {aka int (*)(long int,  void *)}
arrayh5.c:332:6: note: in expansion of macro ‘SUPPRESS_HDF5_ERRORS’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:649:34: note: expected ‘hid_t’ {aka ‘long int’} but argument is of type ‘H5E_auto2_t’ {aka ‘int (*)(long int,  void *)’}
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |                            ~~~~~~^~~~~~~~~
/usr/include/hdf5/serial/H5version.h:782:23: error: too few arguments to function ‘H5Eset_auto2’
  782 |   #define H5Eset_auto H5Eset_auto2
      |                       ^~~~~~~~~~~~
arrayh5.c:44:6: note: in expansion of macro ‘H5Eset_auto’
   44 |      H5Eset_auto(xxxxx_err_func, xxxxx_err_func_data); \
      |      ^~~~~~~~~~~
arrayh5.c:332:6: note: in expansion of macro ‘SUPPRESS_HDF5_ERRORS’
  332 |      SUPPRESS_HDF5_ERRORS(data_id = H5Dopen(id, name));
      |      ^~~~~~~~~~~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:649:15: note: declared here
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |               ^~~~~~~~~~~~
arrayh5.c: In function ‘arrayh5_write’:
/usr/include/hdf5/serial/H5version.h:716:21: error: too few arguments to function ‘H5Dcreate2’
  716 |   #define H5Dcreate H5Dcreate2
      |                     ^~~~~~~~~~
arrayh5.c:363:16: note: in expansion of macro ‘H5Dcreate’
  363 |      data_id = H5Dcreate(file_id, dataname, type_id, space_id, H5P_DEFAULT);
      |                ^~~~~~~~~
/usr/include/hdf5/serial/H5Dpublic.h:322:14: note: declared here
  322 | H5_DLL hid_t H5Dcreate2(hid_t loc_id, const char *name, hid_t type_id, hid_t space_id, hid_t lcpl_id,
      |              ^~~~~~~~~~
make[2]: *** [Makefile:247：arrayh5.o] 错误 1
make[2]: *** 正在等待未完成的任务....
init.c: In function ‘fill_varspace’:
init.c:125:17: error: passing argument 1 of ‘H5Eget_auto2’ makes integer from pointer without a cast [-Wint-conversion]
  125 |     H5Eget_auto(&h5errorfunc, &h5clientdata);
      |                 ^~~~~~~~~~~~
      |                 |
      |                 herr_t (**)(hid_t,  void *) {aka int (**)(long int,  void *)}
In file included from /usr/include/hdf5/serial/hdf5.h:25,
                 from init.c:23:
/usr/include/hdf5/serial/H5Epublic.h:613:34: note: expected ‘hid_t’ {aka ‘long int’} but argument is of type ‘herr_t (**)(hid_t,  void *)’ {aka ‘int (**)(long int,  void *)’}
  613 | H5_DLL herr_t H5Eget_auto2(hid_t estack_id, H5E_auto2_t *func, void **client_data);
      |                            ~~~~~~^~~~~~~~~
init.c:125:31: error: passing argument 2 of ‘H5Eget_auto2’ from incompatible pointer type [-Wincompatible-pointer-types]
  125 |     H5Eget_auto(&h5errorfunc, &h5clientdata);
      |                               ^~~~~~~~~~~~~
      |                               |
      |                               void **
/usr/include/hdf5/serial/H5Epublic.h:613:58: note: expected ‘herr_t (**)(hid_t,  void *)’ {aka ‘int (**)(long int,  void *)’} but argument is of type ‘void **’
  613 | H5_DLL herr_t H5Eget_auto2(hid_t estack_id, H5E_auto2_t *func, void **client_data);
      |                                             ~~~~~~~~~~~~~^~~~
In file included from /usr/include/hdf5/serial/H5public.h:31,
                 from /usr/include/hdf5/serial/hdf5.h:21:
/usr/include/hdf5/serial/H5version.h:749:23: error: too few arguments to function ‘H5Eget_auto2’
  749 |   #define H5Eget_auto H5Eget_auto2
      |                       ^~~~~~~~~~~~
init.c:125:5: note: in expansion of macro ‘H5Eget_auto’
  125 |     H5Eget_auto(&h5errorfunc, &h5clientdata);
      |     ^~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:613:15: note: declared here
  613 | H5_DLL herr_t H5Eget_auto2(hid_t estack_id, H5E_auto2_t *func, void **client_data);
      |               ^~~~~~~~~~~~
init.c:126:17: error: passing argument 1 of ‘H5Eset_auto2’ makes integer from pointer without a cast [-Wint-conversion]
  126 |     H5Eset_auto(NULL,NULL);
      |                 ^~~~
      |                 |
      |                 void *
/usr/include/hdf5/serial/H5Epublic.h:649:34: note: expected ‘hid_t’ {aka ‘long int’} but argument is of type ‘void *’
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |                            ~~~~~~^~~~~~~~~
/usr/include/hdf5/serial/H5version.h:782:23: error: too few arguments to function ‘H5Eset_auto2’
  782 |   #define H5Eset_auto H5Eset_auto2
      |                       ^~~~~~~~~~~~
init.c:126:5: note: in expansion of macro ‘H5Eset_auto’
  126 |     H5Eset_auto(NULL,NULL);
      |     ^~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:649:15: note: declared here
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |               ^~~~~~~~~~~~
init.c:129:17: error: passing argument 1 of ‘H5Eset_auto2’ makes integer from pointer without a cast [-Wint-conversion]
  129 |     H5Eset_auto(h5errorfunc, h5clientdata);
      |                 ^~~~~~~~~~~
      |                 |
      |                 H5E_auto2_t {aka int (*)(long int,  void *)}
/usr/include/hdf5/serial/H5Epublic.h:649:34: note: expected ‘hid_t’ {aka ‘long int’} but argument is of type ‘H5E_auto2_t’ {aka ‘int (*)(long int,  void *)’}
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |                            ~~~~~~^~~~~~~~~
/usr/include/hdf5/serial/H5version.h:782:23: error: too few arguments to function ‘H5Eset_auto2’
  782 |   #define H5Eset_auto H5Eset_auto2
      |                       ^~~~~~~~~~~~
init.c:129:5: note: in expansion of macro ‘H5Eset_auto’
  129 |     H5Eset_auto(h5errorfunc, h5clientdata);
      |     ^~~~~~~~~~~
/usr/include/hdf5/serial/H5Epublic.h:649:15: note: declared here
  649 | H5_DLL herr_t H5Eset_auto2(hid_t estack_id, H5E_auto2_t func, void *client_data);
      |               ^~~~~~~~~~~~
make[2]: *** [Makefile:247：init.o] 错误 1
make[2]: 离开目录“/home/debian/tessa-0.3.1/src”
make[1]: *** [Makefile:177：all] 错误 2
make[1]: 离开目录“/home/debian/tessa-0.3.1/src”
make: *** [Makefile:169：all-recursive] 错误 1

```