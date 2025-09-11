# Install ORSA on RevyOS
## Fetch source code
```bash
git clone https://github.com/AndrewBuck/orsa.git
cd orsa
rm orsa.pri # this is to have the program depend on preinstalled system packages
```

## Install Dependencies

```bash
apt install qtbase5-dev qtbase5-dev-tools qt5-qmake libopenscenegraph-dev libgsl-dev
```

## Modify QMake project file

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

## Building

```bash
qmake
make
```

## Results

Program failed to build due to the following error:

```log
multifit.cpp:837:35: error: ‘struct gsl_multifit_fdfsolver’ has no member named ‘J’
  837 |             gsl_multifit_covar(s->J, 0.0, covar);
      |
```
This is due to incompatible breaking changes in GSL 2.x from 1.x: See https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=805842 for details.
