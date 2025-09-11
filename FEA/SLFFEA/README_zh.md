### **指南：在 RevyOS 上构建 SLFFEA**

本文档提供了在运行 RevyOS 的 RISC-V 设备上从源代码构建 `slffea` 软件的说明。

#### 步骤 1: 获取源代码

```bash
$ wget https://slffea.sourceforge.net/slffea15.tgz
```

#### 步骤 2: 安装依赖并修改 Makefile

- 使用 apt 安装依赖 (包括图形界面):
```
$ sudo apt-get install -y libx11-dev libxext-dev mesa-common-dev libglu1-mesa-devsudo freeglut3-dev libxi-dev libxmu-dev libxt-dev libsm-dev libice-dev
```

- 修改 Makefile:
```
$ find . -name "Makefile" -exec sed -i 's#$(CC) -c#$(CC) $(CFLAGS) -c#g' {} \;
```

#### 步骤 3: 编译

- 无图形界面:
```
make science -j"$(nproc)" \
  CFLAGS='-g -std=gnu89 -fcommon -Wno-implicit-int -Wno-implicit-function-declaration'
```

- 有图形界面:
```
make everything -j"$(nproc)" \
  CFLAGS='-g -std=gnu89 -fcommon -Wno-implicit-int -Wno-implicit-function-declaration'
```

#### 可用性测试

使用源码中自带的测试点进行测试:

```
debian@revyos-lpi4a:~/slffea-1.5/data/br$ ./br
What is the name of the file containing the
brick structural data? (example: rubber)
ball

 Memory requrement for doubles is           69120 bytes

connectivity for element (    23)     23     55     56     24     87    119    120     88  with matl   0
connectivity for element (    24)     24     56     57     25     88    120    121     89  with matl   0
connectivity for element (    25)     25     57     58     26     89    121    122     90  with matl   0
connectivity for element (    26)     26     58     59     27     90    122    123     91  with matl   0
connectivity for element (    27)     27     59     60     28     91    123    124     92  with matl   0
connectivity for element (    28)     28     60     61     29     92    124    125     93  with matl   0
connectivity for element (    29)     29     61     62     30     93    125    126     94  with matl   0
connectivity for element (    30)     30     62     63     31     94    126    127     95  with matl   0
connectivity for element (    31)     31     63     32      0     95    127     96     64  with matl   0
connectivity for element (    32)     64     96     97     65    128    160    161    129  with matl   0
connectivity for element (    33)     65     97     98     66    129    161    162    130  with matl   0
connectivity for element (    34)     66     98     99

connectivity for element (    62)     94    126    127     95    158    190    191    159  with matl   0
connectivity for element (    63)     95    127     96     64    159    191    160    128  with matl   0
connectivity for element (    64)    128    160    161    129    192    224    225    193  with matl   0
connectivity for element (    65)    129    161    162    130    193    225    226    194  with matl   0
connectivity for element (    66)    130    162    163    131    194    226    227    195  with matl   0
connectivity for element (    67)    131    163    164    132    195    227    228    196  with matl   0
connectivity for element (    68)    132    164    165    133    196    228    229    197  with matl   0
connectivity for element (    69)    133    165    166    134    197    229    230    198  with matl   0
connectivity for element (    70)    134    166    167    135    198    230    231    199  with matl   0
connectivity for element (    71)    135    167    168    136    199    231    232    200  with matl   0
connectivity for element (    72)    136    168    169    137    200    232    233    201  with matl   0
connectivity for e

93    261  with matl   0
connectivity for element (   101)    197    229    230    198    261    293    294    262  with matl   0
connectivity for element (   102)    198    230    231    199    262    294    295    263  with matl   0
connectivity for element (   103)    199    231    232    200    263    295    296    264  with matl   0
connectivity for element (   104)    200    232    233    201    264    296    297    265  with matl   0
connectivity for element (   105)    201    233    234    202    265    297    298    266  with matl   0
connectivity for element (   106)    202    234    235    203    266    298    299    267  with matl   0
connectivity for element (   107)    203    235    236    204    267    299    300    268  with matl   0
connectivity for element (   108)    204    236    237    205    268    300    301    269  with matl   0
connectivity for element (   109)    205    237    238    206    269    301    302    270  with matl   0
connectivity for element (   110)    206    238    239    207    270    302    303    271  with matl   0
connectivity for element (   111)    207    239    240    208    271    303    304

force vector for node: (   305)  0.000000e+00   0.000000e+00  -5.000000e+03
force vector for node: (   306)  0.000000e+00   0.000000e+00  -5.000000e+03
force vector for node: (   -10)

stress for node: (   -10)

 We are in case   1

 LU decomposition

 Memory requrement for disp calc. with          333939 doubles is         3130552 bytes

 Which is       2.9855e+00 MB

 This is numel, numel_K, numel_P   224   224     0

 elapsed CPU = 1158.523750
```

证明 `slffea` 在 RevyOS 上具有基本可用性
