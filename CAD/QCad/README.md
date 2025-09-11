### Install QCad on RevyOS

QCAD is a computer-aided design (CAD) software application for 2D design and drafting. It is available for Linux, Apple macOS, Unix and Microsoft Windows.

Currently QCad does not provide binary package for riscv64 architecture, so manually building is required.

#### Building QCad

Install build dependencies:

```shell
sudo apt update
sudo apt install git build-essential gcc make libx11-dev libxext-dev libxrender-dev libglu1-mesa-dev libfreetype6-dev libfontconfig1-dev libssl-dev libdbus-1-dev libsm-dev gcc make libx11-dev libxext-dev libxrender-dev libglu1-mesa-dev libfreetype6-dev libfontconfig1-dev libssl-dev libdbus-1-dev libsm-dev qt5-qmake libqt5svg5-dev libqt5script5 libqt5help5 libqt5designer5 libqt5scripttools5 qtscript5-dev qtxmlpatterns5-dev-tools libqt5xmlpatterns5 libqt5xmlpatterns5-dev libqt5designer5 python3-pyside2.qtuitools libqscintilla2-qt5-designer libqt5designer5 qttools5-dev qt5-image-formats-plugins qtwayland5 qtwayland5-dev-tools libqt5waylandclient5
```

Clone the source code and start compiling:

```shell
git clone https://github.com/qcad/qcad.git --depth=1
cd qcad
# Notice: as Debian Trixie is shipping Qt 5.15.15, this needs to be manually added
mkdir -p src/3rdparty/qt-labs-qtscriptgenerator-5.15.15
cat << 'EOF' > src/3rdparty/qt-labs-qtscriptgenerator-5.15.15/qt-labs-qtscriptgenerator-5.15.15.pro
include( ../../../shared.pri )

SUBDIRS = ../qt-labs-qtscriptgenerator-5.5.0/qtbindings
TEMPLATE = subdirs
EOF
qmakr -r CONFIG+=ractivated
make release -j$(nproc)
```

#### Running on Milk-V Pioneer

Simply run:

```shell
./release/qcad-bin
```

Setup wizard:

![](image/2025-09-11-14-35-08.png)

Main UI:

![](image/2025-09-11-14-35-24.png)