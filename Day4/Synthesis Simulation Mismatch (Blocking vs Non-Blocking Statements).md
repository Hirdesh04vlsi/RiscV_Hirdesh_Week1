# Lab — Synthesis Simulation Mismatch (Blocking / Non-Blocking Statements)  
**Files:** `blocking_caveat.v`, `tb_blocking_caveat.v`

## Objective
- Investigate **synthesis-simulation mismatch** caused by **blocking (`=`) vs non-blocking (`<=`) assignments** in sequential logic.  
- Understand how assignment style affects RTL vs netlist simulation results.

## Steps

### 1️⃣ RTL Simulation

```bash
$ iverilog blocking_caveat.v tb_blocking_caveat.v
$ ./a.out
$ gtkwave dump.vcd
````

<img width="1011" height="489" alt="Screenshot 2025-09-25 224054" src="https://github.com/user-attachments/assets/6edc16d4-fea8-4a92-8e1b-e500d51c53e4" />


### 2️⃣ Synthesis using Yosys

```bash

$ yosys
$ read_liberty -lib ../my_lib/verilog_model/sky130_fd_sc_hd.lib
$ read_verilog blocking_caveat.v
$ synth -top blocking_caveat
$ abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd.lib
$ write_verilog --noattr blocking_caveat_net.v
$ show
$ exit
```
<img width="609" height="310" alt="Screenshot 2025-09-25 224221" src="https://github.com/user-attachments/assets/fc407bfb-301d-4c75-90c1-4e0dc64c1e3a" />


### 3️⃣ Gate-Level Simulation (Netlist Simulation)

```bash
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
$ ./a.out
$ gtkwave dump.vcd
```
<img width="1002" height="481" alt="Screenshot 2025-09-25 224915" src="https://github.com/user-attachments/assets/9ec003e6-6459-43e2-b973-5ebc0236c8e4" />


## Observations

* Synthesis Simulation Mismatch due to Blocking / Non-Blocking Statements.
* Flop Mismatch missing in netlist simulation.
* RTL vs netlist simulation **mismatch** occurs due to **blocking assignments** in sequential logic.
* Changing assignments to **non-blocking (`<=`)** fixes the mismatch.
* Shows how synthesis and netlist handle blocking/non-blocking differently.
