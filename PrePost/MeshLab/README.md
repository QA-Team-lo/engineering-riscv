### **Guide: Installing MeshLab on RevyOS**

This document provides instructions for installing MeshLab, a system for processing and editing triangular meshes, on a RISC-V device running RevyOS. The `meshlab` package is available in the RevyOS repositories, making the installation straightforward.

#### Installation

The installation can be performed using the `apt` package manager.

```bash
debian@revyos-lpi4a:~$ sudo apt install meshlab
[sudo] password for debian: 
Installing:                     
  meshlab

Installing dependencies:
  chemical-mime-data  lib3ds-1-3  libmuparser2v5  libopenctm1

Suggested packages:
  gnome-mime-data

Summary:
  Upgrading: 0, Installing: 5, Removing: 0, Not Upgrading: 0
  Download size: 5,130 kB
  Space needed: 20.8 MB / 89.1 GB available

Continue? [Y/n] y
Get:1 https://fast-mirror.isrc.ac.cn/revyos/revyos-base sid/main riscv64 chemical-mime-data all 0.1.94-7.2 [46.7 kB]
Get:2 https://fast-mirror.isrc.ac.cn/revyos/revyos-base sid/main riscv64 lib3ds-1-3 riscv64 1.3.0-10 [45.4 kB]
Get:3 https://fast-mirror.isrc.ac.cn/revyos/revyos-base sid/main riscv64 libmuparser2v5 riscv64 2.3.3-0.1 [135 kB]
Get:4 https://fast-mirror.isrc.ac.cn/revyos/revyos-base sid/main riscv64 libopenctm1 riscv64 1.0.3+dfsg1-2.1+b3 [47.5 kB]
Get:5 https://fast-mirror.isrc.ac.cn/revyos/revyos-base sid/main riscv64 meshlab riscv64 2020.09+dfsg1-2+b1 [4,856 kB]
Fetched 5,130 kB in 2s (3,127 kB/s)  
Selecting previously unselected package chemical-mime-data.
(Reading database ... 163906 files and directories currently installed.)
Preparing to unpack .../chemical-mime-data_0.1.94-7.2_all.deb ...
Unpacking chemical-mime-data (0.1.94-7.2) ...
Selecting previously unselected package lib3ds-1-3:riscv64.
Preparing to unpack .../lib3ds-1-3_1.3.0-10_riscv64.deb ...
Unpacking lib3ds-1-3:riscv64 (1.3.0-10) ...
Selecting previously unselected package libmuparser2v5:riscv64.
Preparing to unpack .../libmuparser2v5_2.3.3-0.1_riscv64.deb ...
Unpacking libmuparser2v5:riscv64 (2.3.3-0.1) ...
Selecting previously unselected package libopenctm1:riscv64.
Preparing to unpack .../libopenctm1_1.0.3+dfsg1-2.1+b3_riscv64.deb ...
Unpacking libopenctm1:riscv64 (1.0.3+dfsg1-2.1+b3) ...
Selecting previously unselected package meshlab.
Preparing to unpack .../meshlab_2020.09+dfsg1-2+b1_riscv64.deb ...
Unpacking meshlab (2020.09+dfsg1-2+b1) ...
Setting up lib3ds-1-3:riscv64 (1.3.0-10) ...
Setting up chemical-mime-data (0.1.94-7.2) ...
Setting up libopenctm1:riscv64 (1.0.3+dfsg1-2.1+b3) ...
Setting up libmuparser2v5:riscv64 (2.3.3-0.1) ...
Setting up meshlab (2020.09+dfsg1-2+b1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for libc-bin (2.40-3revyos1) ...
Processing triggers for man-db (2.11.2-2) ...
Processing triggers for shared-mime-info (2.2-1+b1) ...
Processing triggers for desktop-file-utils (0.26-1+b1) ...
```

#### Works Fine On Milk-V Pioneer

The installation works fine on the Milk-V Pioneer. Both graphics and rendering work correctly.

```bash
sudo x11vnc -many -display :0 -no6 -rfbport 5900 -auth //run/lightdm/root/:0
```

Using x11vnc to mirror the physical display (with native graphics hardware):

![Works Fine On Milk-V Pioneer](images/03-works-fine-on-milkv-pionner.png)

#### Rendering Issue On LPi4A

Although the installation is successful, MeshLab fails to launch. It reports an error related to GLEW (OpenGL Extension Wrangler Library) initialization, which suggests an issue with the graphics driver or OpenGL context creation on the platform.

![MeshLab refused to launch because of GLEW initialization failure](images/01-meshlab-refuse-to-launch-GLEW-initialization-failed.png)

#### Workarounds for Rendering Issues

To bypass graphics driver issues, I tried one of the following workarounds, but none of them worked:

![Use Zink/LLVMpipe or Disable QT_XCB_INTEGRATION](images/02-meshlab-zink-llvmpipe-or-disable-QT_XCB_INTEGRATION.png)