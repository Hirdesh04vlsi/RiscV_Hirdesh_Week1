
# 📘 VLSI Learning Notes — Day 1

![Simulator: iverilog](https://img.shields.io/badge/Simulator-iverilog-blue)  ![Waveform: GTKWave](https://img.shields.io/badge/Waveform-gtkwave-yellow)  ![Synthesis: Yosys](https://img.shields.io/badge/Synthesis-yosys-green)  ![Library: SKY130](https://img.shields.io/badge/Library-SKY130-red)![RISC-V](https://img.shields.io/badge/ISA-RISC--V-blueviolet)  ![VSD](https://img.shields.io/badge/Company-VSD-brightgreen)  

---

## 1. 🌐 Digital Design & Simulation Basics

### 🔹 What is a Simulator?

A **simulator** models how a digital circuit behaves, without needing to fabricate it.

* **RTL Simulation**: Uses Verilog/SystemVerilog RTL.
* **Gate-Level Simulation**: Uses synthesized netlist + timing info.

### 🔹 Design, Testbench & Stimulus

* **Design (DUT)** → Verilog/SystemVerilog description of the circuit.
* **Testbench (TB)** → Wraps around DUT, generates input signals.
* **Stimulus** → Actual signal patterns applied to DUT.

👉 Example:

* DUT = 2:1 multiplexer.
* TB = applies all possible input combos.
* Stimulus = `sel=0, a=1, b=0` etc.

### 🔹 Waveform Files (.vcd)

* Simulation creates a **VCD (Value Change Dump)** file.
* Records every signal change with timestamp.
* Viewed using **GTKWave**.

---

## 2. 🛠️ Tools Used

| Tool                | Purpose                                                |
| ------------------- | ------------------------------------------------------ |
| **iverilog**        | Verilog/SystemVerilog compiler (simulation).           |
| **vvp** / `./a.out` | Simulation engine (runs compiled design).              |
| **gtkwave**         | GUI waveform viewer for `.vcd` files.                  |
| **yosys**           | Open-source synthesis tool (RTL → netlist).            |
| **.lib files**      | Standard cell libraries: timing, power, functionality. |

---

## 3. 🧪 Lab 1 — Setup

**Directory Structure**

```
sky130RTLDesignAndSynthesisWorkshop/
  ├── DC_WORKSHOP
  ├── lib           # Standard cell libraries (.lib)
  ├── my_lib        # Custom/target libraries
  ├── verilog_files # RTL sources + testbenches
  ├── yosys_run.sh
  └── README.md
```
<img width="1919" height="1032" alt="Screenshot 2025-09-21 232034" src="https://github.com/user-attachments/assets/60d1ba01-b94e-4a47-a3e3-61929255e46e" />

---

## 4. 🧪 Lab 2 — RTL Simulation

**Example: `good_mux`**

```bash
# Compile RTL + TB
iverilog good_mux.v tb_good_mux.v

# Run simulation
./a.out

# View waveform
gtkwave tb_good_mux.vcd
```
<img width="1919" height="454" alt="Screenshot 2025-09-21 233132" src="https://github.com/user-attachments/assets/58eecbf9-7544-46f2-8a6f-face6578caf5" />

Steps in GTKWave:
<img width="991" height="639" alt="Screenshot 2025-09-21 233102" src="https://github.com/user-attachments/assets/f539a8ab-8f6e-4156-aef9-937dbe40e994" />

1. Define UUT (Unit Under Test).
2. Add signals of interest.
3. Observe mux functionality in waveform.

---
View Verilog codes.

```bash
gvim good_mux.v -o tb_good_mux.v
```
<img width="1174" height="1003" alt="Screenshot 2025-09-21 233426" src="https://github.com/user-attachments/assets/154cc9b2-9160-44b8-b0d4-ff12fe3fc725" />


---

## 5. 🔧 Yosys & Logic Synthesis

### 🔹 What is Yosys?

* **Yosys (Yosys Open SYnthesis Suite)** = Open-source logic synthesis tool.
* Converts **RTL design → gate-level netlist** using libraries.
* 

### 🔹 What is Logic Synthesis?

* Mapping behavioral RTL → available gates in a **technology library**.
* Inputs: RTL (.v), .lib (std cell lib).
* Output: gate-level netlist.

### 🔹 Why Verify Netlist?

* To confirm **RTL = Netlist functionality**.
* Same TB can simulate both RTL and netlist.

### 🔹 PPA Tradeoffs (Performance, Power, Area)

* **Fast cells**: low delay, high power, large area.
* **Slow cells**: high delay, low power, small area.
* Libraries have multiple gate flavors for:

  * **Setup-critical paths** → fast cells.
  * **Hold-critical paths** → slower cells.

---

## 6. 🧪 Lab 3 — Yosys Flow

```bash
yosys
```
<img width="1060" height="609" alt="Screenshot 2025-09-19 013924" src="https://github.com/user-attachments/assets/a1e03136-f1db-417e-b921-be0493e08a41" />

Enter yosys shell.

```bash
read_liberty -lib ../path/lib
```
<img width="714" height="230" alt="Screenshot 2025-09-22 001003" src="https://github.com/user-attachments/assets/e6a3300b-f56d-4029-8457-6509b0b746cf" />

Read standard cell library.

```bash
read_verilog good_mux.v
```

Load RTL design.

```bash
synth -top good_mux
```
<img width="961" height="639" alt="Screenshot 2025-09-22 001107" src="https://github.com/user-attachments/assets/17988928-2f0f-4b5b-a7f0-4584699aeb89" />

Run synthesis with `good_mux` as top module.

```bash
abc -liberty ../path/lib
```

Technology mapping: assign logic to actual cells.

```bash
show
```
<img width="958" height="1079" alt="Screenshot 2025-09-22 001321" src="https://github.com/user-attachments/assets/82f64aa0-34a1-4877-aa7a-5a2f5339108a" />
Visual schematic of synthesized design.

```bash
write_verilog good_mux.netlist.v
```

Write synthesized netlist (with attributes).

```bash
!gvim good_mux_netlist.v
```
<img width="1093" height="1079" alt="Screenshot 2025-09-22 002957" src="https://github.com/user-attachments/assets/201d479a-bcf6-4d31-a713-07c729df4a7d" />

Open netlist in `gvim`.

```bash
write_verilog -noattr good_mux_netlist.v
```

Write netlist without synthesis attributes.

```bash
!gvim good_mux_netlist.v
```
<img width="757" height="1079" alt="Screenshot 2025-09-22 003350" src="https://github.com/user-attachments/assets/979c90c2-ec94-4b29-b6e5-1abf9f747e6a" />

View clean netlist.

---

✅ **Day 1 Summary**

* Understood simulators, testbenches, and waveforms.
* Learned about RTL vs gate-level simulation.
* Installed VSDflow + SKY130 libraries.
* Ran RTL simulation using iverilog + gtkwave.
* Introduced yosys and logic synthesis flow.
* Learned PPA tradeoffs and netlist verification.
* Yosys synthesizer

---

## 🙏 Acknowledgement

Thankful to **Kunal Ghosh** & Team **VLSI System Design (VSD)** for designing the lab flow and providing support.

---

## 👤 Author

**Hirdesh**
@hirdeshpamani2@gmail.com
*VLSI Learner & Enthusiast*


