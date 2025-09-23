
# 🧪 Lab: Flip-Flops (Async Reset, Async Set, Sync Reset)

In this lab, we study different types of **D Flip-Flops (DFFs)** and compare their behavior through simulation and synthesis.

---

## 1️⃣ Theory Recap

- **D Flip-Flop** → captures input `D` at the active clock edge and stores it in output `Q`.
- Variants used:
  - **Async Reset (dff_asyncres)**: Reset signal clears output immediately, independent of clock.
  - **Async Set (dff_async_set)**: Set signal drives output high immediately, independent of clock.
  - **Sync Reset (dff_syncres)**: Reset is effective only at the active clock edge.

---

## 2️⃣ Simulation with Icarus Verilog + GTKWave


## Async Reset DFF
```bash
# Async Reset DFF
$ iverilog dff_asyncres.v tb_dff_asyncres.v
$ ./a.out
$ gtkwave tb_dff_asyncres.vcd &

```
<img width="1116" height="1079" alt="Screenshot 2025-09-24 031727" src="https://github.com/user-attachments/assets/b4b03940-3152-43cb-a54f-5c27b69a9b09" />


---

## 3️⃣ Synthesis with Yosys

We now synthesize the flip-flops using the SKY130 standard cell library.

```bash
$ yosys
yosys> read_liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib

# Async Reset DFF
yosys> read_verilog dff_asyncres.v
yosys> synth -top dff_asyncres
yosys> dfflibmap -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr dff_asyncres_netlist.v
yosys> clean
```
<img width="1122" height="665" alt="Screenshot 2025-09-24 033902" src="https://github.com/user-attachments/assets/02e2da76-3e53-4f80-bef0-2d54bf39c522" />


---


## Async Set DFF
```bash

# Async Set DFF
$ iverilog dff_async_set.v tb_dff_async_set.v 
$ ./a.out
$ gtkwave tb_dff_async_set.vcd &
```
<img width="1003" height="657" alt="Screenshot 2025-09-24 032520" src="https://github.com/user-attachments/assets/2c78701e-3f91-4a56-8d52-179b17f33022" />


---


```bash
# Async Set DFF
yosys> read_verilog dff_async_set.v
yosys> synth -top dff_async_set
yosys> dfflibmap -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr dff_async_set_netlist.v
yosys> clean
```

<img width="1079" height="625" alt="Screenshot 2025-09-24 034219" src="https://github.com/user-attachments/assets/f8342078-e33a-4057-8dec-f87c19e0f45b" />

---




## Sync Reset DFF

```bash

# Sync Reset DFF
$ iverilog dff_syncres.v tb_dff_syncres.v 
$ ./a.out
$ gtkwave tb_dff_syncres.vcd &
````
<img width="1119" height="1078" alt="Screenshot 2025-09-24 032657" src="https://github.com/user-attachments/assets/e2ae1adc-b680-499c-a756-023653d091db" />


---


```bash
# Sync Reset DFF
yosys> read_verilog dff_syncres.v
yosys> synth -top dff_syncres
yosys> dfflibmap -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr dff_syncres_netlist.v
```
<img width="951" height="1017" alt="Screenshot 2025-09-24 034526" src="https://github.com/user-attachments/assets/8b836a73-7512-4688-8569-4aabc678764d" />


---

✅ **Observation in GTKWave**

* Async reset/set → Output changes immediately with control signals.
* Sync reset → Output changes only on clock edge.

✅ **Observation in yosys:**

* Yosys maps flip-flops to standard library DFF cells with corresponding reset/set pins.
* Optimizations depend on available cells in the SKY130 library.

---

## 📂 Outputs

* Simulation waveforms (`.vcd`) for each testbench.
* Gate-level netlists (`_netlist.v`) for each design.

---
