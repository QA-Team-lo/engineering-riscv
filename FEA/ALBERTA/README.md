### **Guide: Building ALBERTA on RevyOS**

This document provides instructions for building the `ALBERTA` software from source on a RISC-V device running RevyOS.

#### **Step 1: Install Dependencies**

First, ensure that basic build tools and autotools are installed:

```bash
$ sudo apt update
$ sudo apt install build-essential automake autoconf libtool pkg-config
$ sudo apt install libtirpc-dev
```

#### **Step 2: Generate the `configure` Script**

```bash
$ ./generate-alberta-automakefiles.sh
$ autoreconf --force --install
```

After this step, a `configure` file will be generated in the directory:

```bash
$ ./configure
```

#### **Step 3: Build and Install**

```bash
$ make -j$(nproc)
$ sudo make install
```

#### **Step 4: Verify Installation**

```bash
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
```

```bash
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

This confirms that `ALBERTA` has basic functionality on RevyOS.
