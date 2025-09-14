# 指南：在 RevyOS 上构建 ORSA

本文档提供了在运行 RevyOS 的 RISC-V 设备上构建 ORSA 软件的说明。

## 第一步: 获取源码
```bash
git clone https://github.com/AndrewBuck/orsa.git
cd orsa
rm orsa.pri # this is to have the program depend on preinstalled system packages
```

## 第二步: 安装依赖

```bash
apt install qtbase5-dev qtbase5-dev-tools qt5-qmake libopenscenegraph-dev libgsl-dev
```

## 第三步: 修改QMake项目文件

```diff
diff --git a/orsa.pro b/orsa.pro
index 2f52aab..f3e412f 100644
--- a/orsa.pro
+++ b/orsa.pro
@@ -1,9 +1,7 @@
-include(orsa.pri)
-
 TEMPLATE = subdirs

 SUBDIRS += src app

 CONFIG += ordered

-DISTFILES += orsa.pro orsa.pri
+DISTFILES += orsa.pro
```

## 第四步: 构建

```bash
qmake
make
```

## Results

由于以下错误, 程序无法构建:

```log
multifit.cpp:837:35: error: ‘struct gsl_multifit_fdfsolver’ has no member named ‘J’
  837 |             gsl_multifit_covar(s->J, 0.0, covar);
      |
```
这是由于 GSL 2.x 与 1.x 不兼容的重大变化造成的: 有关详细信息, 请参阅 https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=805842