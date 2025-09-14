### **指南：在 RevyOS 上构建 ALBERTA**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `ALBERTA` 软件的说明。

#### 步骤 1: 安装依赖
使用 `apt` 包管理器安装以下软件包

```bash
sudo apt update
sudo apt install build-essential automake autoconf libtool pkg-config
sudo apt install libtirpc-dev
```

#### 步骤 2: 生成 configure 脚本

```bash
./generate-alberta-automakefiles.sh
autoreconf --force --install
```

之后目录里会生成 `configure` 文件

```bash
./configure
```

#### 步骤 3: 编译并安装

```bash
make -j$(nproc)
sudo make install
```

#### 步骤 4: 安装验证
```
debian@revyos-lpi4a:~/alberta3-main$ ./add_ons/gmv/2d/alberta2gmv2d --help

  -s, --scalar=DRV
            Load the data-file DRV which must contain a DOF_REAL_VEC
            dumped to disk by `write_dof_real_vec[_xdr]()'.
            This option may be specified multiple times. The DOF_REAL_VECs
            must belong to the most recently specified mesh.
            See `-m' and `-b' above.
  -v, --vector=DRDV
            Load the data-file DRDV which must contain a DOF_REAL_D_VEC
            dumped to disk by `write_dof_real_d_vec[_xdr]()'.
            This option may be specified multiple times. The vector
            must belong to the most recently specified mesh.
            See `-m' and `-b' above.
  -o, --output=FILENAME
            Specify an output file-name. If this option is omitted, then the
            output file-name is "alberta.gmv".
  -p, --path=PATH
            Specify a path prefix for all following data-files. This option
            may be specified multiple times. PATH is supposed to be the
            directory containing all data-files specified by the following
            `-m', `-s' and `-v' options.
  -h, --help
            Print this help.
debian@revyos-lpi4a:~/alberta3-main$ ./add_ons/gmv/1d/alberta2gmv1d

  -s, --scalar=DRV
            Load the data-file DRV which must contain a DOF_REAL_VEC
            dumped to disk by `write_dof_real_vec[_xdr]()'.
            This option may be specified multiple times. The DOF_REAL_VECs
            must belong to the most recently specified mesh.
            See `-m' and `-b' above.
  -v, --vector=DRDV
            Load the data-file DRDV which must contain a DOF_REAL_D_VEC
            dumped to disk by `write_dof_real_d_vec[_xdr]()'.
            This option may be specified multiple times. The vector
            must belong to the most recently specified mesh.
            See `-m' and `-b' above.
  -o, --output=FILENAME
            Specify an output file-name. If this option is omitted, then the
            output file-name is "alberta.gmv".
  -p, --path=PATH
            Specify a path prefix for all following data-files. This option
            may be specified multiple times. PATH is supposed to be the
            directory containing all data-files specified by the following
            `-m', `-s' and `-v' options.
  -h, --help
            Print this help.
```

证明 `ALBERTA` 在 RevyOS 上具有基本可用性
