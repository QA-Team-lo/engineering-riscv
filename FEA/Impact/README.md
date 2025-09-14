### **Guide: Installing Impact on RevyOS**

This document provides instructions for installing `Impact` on a RISC-V device running RevyOS.
`Impact` is software developed in Java, and since precompiled versions are available on its official website, installation is very straightforward.

#### Installation

Install dependencies using `apt`:

```bash
sudo apt install openjdk-17-jdk
```

Download the files from the official `Impact` website:

```bash
wget https://master.dl.sourceforge.net/project/impact/impact/Version%200.7.06.042/Impact-0.7.06.042.zip?viasf=1
mv 'Impact-0.7.06.042.zip?viasf=1' Impact.zip
unzip Impact.zip
```

Then grant execution permissions to the script:

```bash
chmod +x ./Impact.sh
```

#### Availability Test

Run the built-in test case:

```bash
debian@revyos-lpi4a:~/Impact$ ./Impact.sh examples/cylinder.in

=========== Welcome to Impact ==========
= Impact is free software and licensed =
= under GPL. Please note that it comes =
= with no warranties whatsoever.       =
=                                      =
= Report any bugs at our webpage:      =
= http://impact.sourceforge.net        =
=============== ENJOY!!!! ==============


*******************************
*** Solver has been invoked ***
*******************************


Running on 4 clients


Processing file: examples/cylinder.in
*** Initializing ***
Found 1392 elements
Found 1 trackers
Found 1 materials
Found 723 nodes
Found 2 constraints
Found 0 loads
Found 2 controls
Found 0 groups
Reading Constraints
Constraint block found!
Filled constraintlist
Reading Loads
Filled loadlist
Reading Nodes
10.0% complete
20.0% complete
30.0% complete
40.0% complete
50.0% complete
60.0% complete
70.0% complete
80.0% complete
90.0% complete
Filled nodelist
Reading Materials
Block found!
Filled materiallist
Reading Elements
Element block found!
10.0% complete
20.0% complete
30.0% complete
40.0% complete
50.0% complete
60.0% complete
70.0% complete
80.0% complete
90.0% complete
Element block found!
Filled elementlist
Reading Trackers
Tracker block found!
Filled trackerlist
Reading Controls
Filled controlset
*** Assembling the Mass Matrix ***
Assembling Elements
Assembling Nodes
Assembling Constraints
*** Setting Initial Conditions ***
Initializing nodes
Sorting nodes
Setting up node neighbour handles
Determining optimal model distribution for nodes
Initializing elements
Assigning each element to a CPU
Initializing trackers
Initializing constraints
Determining smallest timestep size
Determined timestep: 4.8293633530785456E-4
Calculating total model mass
Total model mass = 0.24444260770968435
*** Initiating Solver ***
Distributing the model on 4 CPU:s
CPU 0 Nodes: 181        Elements: 338
CPU 1 Nodes: 181        Elements: 362
CPU 2 Nodes: 181        Elements: 362
CPU 3 Nodes: 180        Elements: 330


Time: 0.0       Remaining time (hh:mm:ss) 00:00:39
......
```
