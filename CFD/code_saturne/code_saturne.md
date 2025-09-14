# Code Saturne (CFD) on RevyOS

## Prerequisites

See the [offical installation document](https://github.com/code-saturne/code_saturne/blob/master/INSTALL.md)

```
sudo apt install python3-pyqt5 pyqt5-dev-tools gfortran
```

## Obtain the Source Code

```
wget https://www.code-saturne.org/releases/code_saturne-9.0.0.tar.gz
tar -xzvf code_saturne-9.0.0.tar.gz
```

## Compile and Install

```
mkdir code_saturne_build
cd code_saturne_build
mkdir prod
cd prod
../../code_saturne-9.0.0/configure
make
sudo make install
```

## Verification

![](./code_saturne.png)
