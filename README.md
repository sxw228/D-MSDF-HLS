# Dynamatic & MSDF HLS

## Dynamically Scheduled MSDF High-Level Synthesis 

This project is inspired by Dynamatic and we focus on HLS for MSDF Based Arbitrary Precision Computing.
* **elastic-circuits**: LLVM Passes presented here wiil be used as the Front end of DMSDF HLS.

### Compiler

The DHLS's compilation flow consists of the following parts:

* **elastic-circuits**: A collection of LLVM passes which input a C/C++ program and transform its IR into a dataflow circuit. The output of this pass is a .DOT file which represents a netlist of dataflow components. The details of the .DOT dataflow description are given in *Documents/Dataflow.md*.

* **Buffers**: Buffer placement for throughput and critical path optimization. Inputs the .DOT netlist from *elastic-circuits* and outputs a .DOT netlist with buffers placed and sized to maximize throughput under a given clock period constaint. 

* **dot2hdl**: Inputs a .DOT netlist (either produced by *elastic-circuits*, or by *Buffers*) and outputs an equivalent VHDL netlist of dataflow components. VHDL descriptions of the dataflow components are given in *components*. Apart from the VHDL netlist, *dot2vhdl* also produces a .json file for configuring LSQs (if required by the design). The LSQ can be configured using the files in *chisel_lsq*.

All folders contain README files with detailed instructions on installation and running the code.

### Simulation and Verification

Please refer to *Documents/ModelSim Simulation.md* on details about running the simulation and incorporating Xilinx computational units into the dataflow designs. The designs can be verified using the framework in the *hls_verifier* folder. 
