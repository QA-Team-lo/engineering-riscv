# TYPHON (CFD) on RevyOS

## Obtain the Source Code

```
curl -L "https://sourceforge.net/projects/typhon/files/typhon-solver/0.5.0/typhon-solver-0.5.0-sources.tar.gz/download" -o typhon-solver-0.5.0-sources.tar.gz
tar -zxvf typhon-solver-0.5.0-sources.tar.gz
```

## Compile and Install

The installation fails due to errors within the install.sh.

```
debian@revyos-pioneer:~/CFD/typhon$ ./install.sh
---------------------------------------------------------------------
TYPHON installation

./install.sh: 9: [: debian: unexpected operator
. regular user installation

./install.sh: 23: [[: not found
Please choose and check a (writable) path of TYPHON installation:
[/home/debian/.local] ?
./install.sh: 23: [[: not found
Please choose and check a (writable) path of TYPHON installation:
[/home/debian/.local] ? ^C
```
