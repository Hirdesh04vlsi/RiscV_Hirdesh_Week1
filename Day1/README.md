
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


---


## 3. 🔧 Yosys & Logic Synthesis

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


## 🧪 Day 1 Labs

- [Lab 1: Tool Installation](./LAB%201%20Tool%20installation.md)  
- [Lab 2: Icarus Verilog + GTKWave](./LAB%202%20iverilog_gtkwave.md)  
- [Lab 3: Yosys Basics](./LAB%203%20Yosys.md)  

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


