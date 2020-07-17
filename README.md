k## Under construction (more to come)

<img src="./docs/baseline.gif" alt="Alternate Text" /></a>
## Required Packages:
	- readline: sudo apt-get install libreadline6 libreadline6-dev (Ubuntu)
	- readline: sudo yum install readline-devel (CentOS 7)
	- -stdc++11 
	- OpenMP (https://www.openmp.org/resources/openmp-compilers-tools/)
		- wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.1.tar.gz; tar -xvf openmpi-4.0.1.tar.gz
		- cd openmpi-4.0.1; ./configure; make -j12 all install
	- Yosys -- https://github.com/YosysHQ/yosys
	- export "abc" and "yosys" PATH TO .bashrc to have a global access
		- Instruction:  echo "export PATH=$(pwd):\${PATH}" >> ~/.bashrc; source ~/.bashrc
		- Testing : If PATH added succesfully, you should be able to type "abc" and "yosys" at any location of your LINUX system 
	
## "ftune" implemented in ABC

	abc 01> ftune -h
	usage: ftune [drtfpihSL]
		flowtune(ftune): Multi-Armed Bandit (MAB) synthesis flow tuning for Logic Minimization
	-t    : Targeted metric (default = 0, i.e., AIG Minization targeting number of AIG nodes)
	      : t=0 AIG Minization - Minimizing AIG nodes                       :  ftune -d i10.aig -r 4 -t 0 -p 1 -i 10 -s 5 -L [other options]
	      : t=1 AIG Minization - minimizing AIG levels                      :  ftune -d i10.aig -r 4 -t 1 -p 1 -i 10 -s 5 -L [other options]
	      : t=2 Technology mapping (w Gate Sizing + STA) Min STA-Delay      :  ftune -d i10.aig -r 4 -t 2 -p 1 -i 10 -s 5 -L your.lib(Liberty) [other options]
	      : t=3 Technology mapping (w Gate Sizing + STA) Min Area           :  ftune -d i10.aig -r 4 -t 3 -p 1 -i 10 -s 5 -L your.lib(Liberty) [other options]
	      : t=4 FPGA Mapping - Miniziming Number of 6-input LUTs            :  ftune -d i10.aig -r 4 -t 4 -p 1 -i 10 -s 5 [other options]
	      : t=5 FPGA Mapping - Miniziming Levels of 6-input LUT network     :  ftune -d i10.aig -r 4 -t 5 -p 1 -i 10 -s 5 [other options]
	      : t=6 SAT (CNF) Minization - Miniziming Number of Clauses         :  ftune -d cnf.aig -r 4 -t 6 -p 1 -i 10 -s 5 [other options]
	      : t=7 SAT (CNF) Minization - Miniziming Number of literals        :  ftune -d cnf.aig -r 4 -t 7 -p 1 -i 10 -s 5 [other options]
	      : t=8 Regular Technology mapping using GENLIB (map) Min Delay     :  ftune -d i10.aig -r 4 -t 8 -p 1 -i 10 -s 5 -L your.genlib(GENLIB) [other options]
	      : t=9 Regular Technology mapping using GENLIB (map) Min Area      :  ftune -d i10.aig -r 4 -t 9 -p 1 -i 10 -s 5 -L your.genlib(GENLIB) [other options]
	-h    : Print the command usage
	-r    : Number of appearances of each synthesis command (default = 3)
	-p    : Factor of number of Arms (default = 1)
	-i    : Number of MAB iterations (default = 10)
	-s    : Number of MAB sampling for each iteration per Arm (default = 5)
	-f    : Toggle using long-short-term memory for probability matching (default=FALSE)
	-C    : Customize the logic transformations (commands) for tuning instead of default commands. 
		 Example 1:  -C b;rw;rwz;rf (your flow will use these 4 opts).
 			
		 Example 2: -C b;rw;rwz;rf;your_cmd (You can define your new command in abc.rc namely your_cmd and tunes for these 4 opts + your_cmd).
	-S    : Toggle using Softmax() or LogSoftmax() for finalizing samples at each iteration (default=FALSE)
		    -S 0 : Using winning rate Pr
		    -S 1 : Using Softmax()
		    -S 2 : Using LogSoftmax()
	-L    : Liberty or GENLIB file. Required for Technology mapping tuning (t=2,3,8,9) 
## VTR (vtr-verilog-to-routing) Integration
See <b><i>./FlowTune-Integration-VTR/ftune_vtr_flow.pl</i></b> for details

### Comparisons between default VTR vs FTune VTR using "bfly.v" (Architecture = k6\_frac\_N10\_frac\_chain\_mem32K\_40nm")

```

```


<b>Post Place-and-Route</b> Circuit Statistics with FlowTune: 
```
./ftune_vtr_flow.pl bfly.v k6_frac_N10_frac_chain_mem32K_40nm.xml 
```

```
  Blocks: 11877
    .input  :     193
    .latch  :    2296
    .output :      64
    0-LUT   :      15
    6-LUT   :    8285
    adder   :    1020
    multiply:       4
  Nets  : 12861
    Avg Fanout:     3.4
    Max Fanout:  2296.0
    Min Fanout:     1.0
  Netlist Clocks: 1
 Build Timing Graph
  Timing Graph Nodes: 56537
  Timing Graph Edges: 98892
  Timing Graph Levels: 116
```

<b>Post Place-and-Route</b> Circuit Statistics without FlowTune: 
```
./run_vtr_flow.pl bfly.v k6_frac_N10_frac_chain_mem32K_40nm.xml
```

```
  Blocks: 13921
    .input  :     193
    .latch  :    2296
    .output :      64
    0-LUT   :      15
    6-LUT   :   10329
    adder   :    1020
    multiply:       4
  Nets  : 14905
    Avg Fanout:     3.7
    Max Fanout:  2296.0
    Min Fanout:     1.0
  Netlist Clocks: 1
# Build Timing Graph
  Timing Graph Nodes: 70763
  Timing Graph Edges: 123256
  Timing Graph Levels: 112
```



