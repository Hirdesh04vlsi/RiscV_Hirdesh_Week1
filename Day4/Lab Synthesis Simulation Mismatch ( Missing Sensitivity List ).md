# Lab — Synthesis Simulation Mismatch (Missing Sensitivity List)  
**Files:** `bad_mux.v`, `tb_bad_mux.v`

## Objective
- Observe **synthesis-GLS mismatch** caused by missing signals in the always block sensitivity list.  
- Understand why combinational logic can behave like flip-flops when signals are missing.

## Steps

### 1️⃣ RTL Simulation
```bash
$ gvim bad_mux.v
$ iverilog bad_mux.v tb_bad_mux.v
$ ./a.out
$ gtkwave dump.vcd
````
<img width="586" height="325" alt="Screenshot 2025-09-25 022512" src="https://github.com/user-attachments/assets/5374ee35-0e38-4d56-b7d5-1fcd20c5e176" />
<img width="1002" height="380" alt="Screenshot 2025-09-25 223102" src="https://github.com/user-attachments/assets/4eaf33cc-b3b5-4566-bffb-df7b8f96d467" />


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

<img width="615" height="369" alt="Screenshot 2025-09-25 222633" src="https://github.com/user-attachments/assets/7572e51c-5a2e-4efc-84b2-7e3a2c54e3fb" />


### 3️⃣ Gate-Level Simulation (Netlist Simulation)

```bash
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
$ ./a.out
$ gtkwave dump.vcd
```
<img width="999" height="291" alt="Screenshot 2025-09-25 222134" src="https://github.com/user-attachments/assets/4e99c2aa-5686-4367-b0a1-6b1eb55ff9a6" />


## Observations

* **Synthesis Simulation Mismatch** due to **Missing Sensitivity List**.
* RTL vs Netlist outputs **mismatch**.
* Combinational mux behaves like a flip-flop due to **missing sensitivity list**.

