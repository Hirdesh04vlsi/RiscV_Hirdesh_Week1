# Lab GLS — Gate-Level Simulation  
**Files:** `ternary_operator.v`, `tb_ternary_operator.v` , `ternary_operator_net.v`

## Objective
- Run **gate-level simulation** on the synthesized netlist.  
- Verify logical correctness after synthesis.  

## Steps

### 1️⃣ RTL Simulation
```bash
$ iverilog ternary_operator.v tb_ternary_operator.v
$ ./a.out
$ gtkwave dump.vcd
````
<img width="1001" height="467" alt="Screenshot 2025-09-25 220547" src="https://github.com/user-attachments/assets/6d4f5907-0158-41a3-995e-73c860e8f871" />


### 2️⃣ Synthesis using Yosys

```bash
$ yosys
$ read_liberty -lib ../path.lib
$ read_verilog ternary_operator.v
$ synth -top ternary_operator
$ abc -liberty ../path.lib
$ write_verilog --noattr ternary_operator_net.v
$ show
$ exit
```
<img width="626" height="425" alt="Screenshot 2025-09-25 221121" src="https://github.com/user-attachments/assets/9a7a242f-c0e0-418b-b712-08e5f6851e07" />


### 3️⃣ Gate-Level Simulation (Netlist Simulation)

```bash
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_net.v tb_ternary_operator.v
$ ./a.out
$ gtkwave dump.vcd
```
<img width="1003" height="382" alt="Screenshot 2025-09-25 223504" src="https://github.com/user-attachments/assets/ac20bdbc-30d4-43da-bf91-df1a55c66595" />


## Observations

* Compare GTKWave outputs from RTL and netlist simulation.
* Outputs match → GLS confirms logical correctness.

````
