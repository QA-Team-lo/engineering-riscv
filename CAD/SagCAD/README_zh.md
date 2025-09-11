### 在 RevyOS 上安装 SagCAD

SagCAD 是一款简单易用的 2D CAD/CAM 软件。

#### 安装 SagCAD

Debian/RevyOS 并未提供 SagCAD 打包，可以考虑下载 Ubuntu 版本并手动运行。

警告：请勿尝试在 Debian 系统上安装 Ubuntu 或其他发行版的 deb 包，这会导致意料之外的情况并可能损坏系统！

```shell
mkdir sagcad; cd sagcad
wget http://launchpadlibrarian.net/722730855/sagcad_0.9.14-0ubuntu6_riscv64.deb
dpkg-deb -x sagcad_0.9.14-0ubuntu6_riscv64.deb .
```

#### 在 Milk-V Pioneer 上运行 SagCAD

```shell
./usr/bin/sagcad
```

在 Milk-V Pioneer 上可以正常运行。

![](image/2025-09-11-15-48-43.png)