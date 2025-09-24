
# 📘 VLSI Learning Notes — Day 2

![Simulator: iverilog](https://img.shields.io/badge/Simulator-iverilog-blue)  ![Waveform: GTKWave](https://img.shields.io/badge/Waveform-gtkwave-yellow)  ![Synthesis: Yosys](https://img.shields.io/badge/Synthesis-yosys-green)  ![Library: SKY130](https://img.shields.io/badge/Library-SKY130-red)

---

## 📝 Topics Covered

### 🔹 Standard Cell Library (.lib)

In digital design, the **Standard Cell Library** (`.lib`) is the heart of synthesis.  
It describes the logical and timing characteristics of every gate/cell available in a technology node.

- **Contents of a .lib file**
  - Functional description of cells (NAND, NOR, XOR, Flip-Flops, etc.)
  - Different drive strengths (e.g., `INV_X1`, `INV_X4` → same inverter with different sizes)
  - Timing data: propagation delay, setup/hold times, etc.
  - Power data: leakage and dynamic power for each cell.
  - Area and capacitance values.

- **Naming convention example:**
```

sky130_fd_sc_hd__tt_025C_1v80.lib

````
- `sky130_fd_sc_hd` → technology and library name (Sky130, Foundry Digital, High Density)  
- `tt` → **typical process corner**  
- `025C` → operating temperature (25°C)  
- `1v80` → supply voltage (1.8 V)

So this file is basically the **dictionary** the synthesis tool uses to translate RTL into gates.
<img width="960" height="1079" alt="Screenshot 2025-09-24 005528" src="https://github.com/user-attachments/assets/fd1be931-f6a4-44f0-9fff-74b1866c321c" />

---

### 🔹 PVT Corners (Process, Voltage, Temperature)

The performance of transistors is not constant — it changes with:
- **Process variation (P):** manufacturing differences → devices can be *fast* (ff), *slow* (ss), or *typical* (tt).
- **Voltage (V):** higher supply voltages → faster switching but more power; lower → slower but lower power.
- **Temperature (T):** higher temperatures slow devices down; low temps can make them faster.

**Why PVT matters in VLSI:**
- Designs must work across *all* corners, not just at one.
- For example:  
- `ss_125C_1v60` → worst-case corner (slow, hot, low V).  
- `ff_n40C_1v95` → best-case corner (fast, cold, high V).  

Designers must ensure **timing closure** across these.

---

### 🔹 Hierarchical vs Flat Synthesis

- **Hierarchical synthesis:**
- Each submodule is synthesized separately.
- Preserves design readability, debugging, and reuse.
- Useful for very large designs.

- **Flat synthesis:**
- All modules are flattened into a single netlist.
- The tool sees everything at once → can optimize across module boundaries.
- Results in better optimization, but harder to debug.

**Lab 4 Work:**
---
## 🧪 Labs

- [Hierarchical vs Flat Synthesis (with Submodule Synthesis)](./Hierarchical%20vs%20Flat%20lab.md)

- File: `multiple_modules.v
- Commands used:
```tcl
read_liberty <.lib>
read_verilog multiple_modules.v
synth -top <module>
abc -liberty <.lib>
show
write_verilog -noattr
show
````

* Observations:

  * Hierarchical → netlist shows submodules clearly.
  * Flat → everything merged into a single blob of gates.

---

### 🔹 NAND vs NOR Implementation

Why are **NAND gates preferred** in digital design libraries?

* **Stacking effect:**

  * NOR requires stacking multiple PMOS transistors in series.
  * PMOS has lower mobility compared to NMOS → stacking worsens performance.
* NAND requires stacking NMOS (better mobility), so it’s faster and more area/power efficient.

This is why synthesis often chooses **NAND-based implementations**.

---

### 🔹 Submodule Synthesis

In complex SoCs, modules may be reused thousands of times.
Synthesizing submodules separately:

* Saves synthesis time (compile once, reuse).
* Allows optimization at the sub-block level.
* Keeps hierarchy intact for debugging and verification.

Lab: Synthesized both top and individual submodules of `multiple_modules.v`.

---

### 🔹 Flip-Flops

**Flip-flops are the memory elements of sequential logic.**
They store state information and synchronize data with the clock.

Types studied:

* `dff_asyncres.v` → DFF with asynchronous reset.
* `dff_async_set.v` → DFF with asynchronous set.
* `dff_syncres.v` → DFF with synchronous reset.

**Lab 5 Flow:**

- [D Flip-Flop Lab (Async Reset, Async Set, Sync Reset)](./D_FlipFlop_Lab.md)

1. Simulate using Icarus Verilog + GTKWave:

   ```bash
   iverilog dff_asyncres.v
   ./a.out
   gtkwave dump.vcd
   ```
2. Run through Yosys for synthesis:

   ```tcl
   read_liberty <.lib>
   read_verilog dff_asyncres.v
   synth -top dff_asyncres
   dfflibmap -liberty <.lib>
   abc -liberty <.lib>
   show
   ```

---
### 🔹 Optimizations in Yosys

One of the strengths of synthesis tools is **optimization**.  
Yosys can detect simple arithmetic patterns and replace them with efficient wiring or logic.

* **Example 1: Multiplication by 2 (`mult_2.v`)**
  * Instead of a real multiplier, Yosys implements a **bit shift**:  
    `y = {a, 1'b0}`

* **Example 2: Multiplication by 9 (`mult_8.v`)**
  * Code used concatenation: `y = {a, a}`  
  * This is equivalent to `(a << 3) + a = a * 9`  
  * Yosys implements this as **shift + add**, not a full multiplier.

✅ Result → No complex multiplier hardware is created → **area and power savings**.

---

### 🧪 Lab 6 Flow


## 📂 Files

* `multiple_modules.v` → hierarchical vs flat synthesis lab
* `dff_asyncres.v`, `dff_async_set.v`, `dff_syncres.v` → flip-flop labs
* `mult_2.v`, `mult_8.v` → optimization examples

---

## ✍️ Author

**Hirdesh**
VLSI Enthusiast | RTL to GDS Learner | Open-Source EDA Explorer 🚀

```




