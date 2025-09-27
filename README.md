
# ![RISC-V](https://img.shields.io/badge/RISC--V-VSD-orange) ![Made in India](https://img.shields.io/badge/Made%20in-India-green) ![VSD Workshop](https://img.shields.io/badge/VSD-Workshop-blue) ![RTL](https://img.shields.io/badge/RTL-Design-red) ![Synthesis](https://img.shields.io/badge/Synthesis-Green) ![FPGA](https://img.shields.io/badge/FPGA-Design-purple) ![ASIC](https://img.shields.io/badge/ASIC-Flow-yellow) ![Verilog](https://img.shields.io/badge/Verilog-HDL-blue) ![Simulation](https://img.shields.io/badge/Simulation-✔️-lightgrey) ![Optimization](https://img.shields.io/badge/Optimization-✓-orange)

# VSD RISC-V Silicon to Tapeout – Week 1 Summary

---

## [Day 1 – Tool Installation & Setup](./Day1)

**Learnings:**

* Installed and verified **Icarus Verilog, GTKWave, and Yosys**.
* Learned the basic **simulation and synthesis workflow**.

**Labs:**

* [**LAB 1: Tool Installation**](./Day1/LAB%201%20Tool%20installation.md)
* [**LAB 2: Icarus Verilog + GTKWave**](./Day1/LAB%202%20iverilog_gtkwave.md)
* [**LAB 3: Yosys Basics**](./Day1/LAB%203%20Yosys.md)

**Observation:** Tools ready for RTL simulation and synthesis.

---

## [Day 2 – Sequential Circuits & Flip-Flops](./Day2)

**Learnings:**

* Designed and simulated **D Flip-Flops**.
* Learned **synchronous design principles**.
* Explored **multiplication optimization** and logic reduction techniques.

**Labs:**

* [**Lab 4: Hierarchical vs Flat Synthesis**](./Day2/Hierarchical_vs_Flat_Synthesis.md)
* [**Lab 5: Flip-Flops (Async Reset, Async Set, Sync Reset)**](./Day2/D_Flip%20Flop%20Lab.md)
* [**Lab 6: Multiplication Optimization**](./Day2/Multiplication%20Optimization%20Lab.md)

**Observation:** Flip-flops behave correctly; combinational optimization reduces logic usage.

---

## [Day 3 – Combinational Optimization](./Day3)

**Learnings:**

* Learned how **Yosys optimizes combinational circuits**.
* Observed **removal of redundant logic** and cleaner netlists.

**Labs:**

* [**Lab: Combinational Optimization**](./Day3/Lab%20Combinational%20optimization.md)
* [**Lab: Sequential Optimization**](./Day3/Lab%20Sequential%20optimization.md)
* [**Lab: Unused Output Optimization**](./Day3/Lab%20Unused%20output%20optimization.md)

**Observation:** Optimized circuits consume less logic and produce same functionality.

---

## [Day 4 – Gate-Level Simulation & Sensitivity List](./Day4)

**Learnings:**

* Performed **gate-level simulations** and compared with RTL.
* Learned importance of **complete sensitivity lists** in always blocks.
* Observed **simulation-synthesis mismatches** due to missing sensitivity entries.

**Labs:**

* [**Lab GLS (Gate-Level Simulation)**](./Day4/Lab%20GLS%20%28gate%20level%20simulation%29.md)
* [**Lab (Missing Sensitivity List) Synthesis Simulation Mismatch**](./Day4/Lab%20%28Missing%20Sensitivity%20List%29%20Synthesis%20Simulation%20Mismatch.md)
* [**Lab (Blocking vs Non-Blocking Statements) Synthesis Simulation Mismatch**](./Day4/Lab%20%28Blocking%20vs%20Non-Blocking%20Statements%29%20Synthesis%20Simulation%20Mismatch.md)

**Observation:** Gate-level simulation matches RTL; missing sensitivity list can cause mismatches.

---

## [Day 5 – If-Else, Case Constructs, and Loops](./Day5)

**Learnings:**

* **If / If-Else statements:** Mapped to **MUX hardware**, missing `else` can infer latches.
* **Case statements:** Complete vs incomplete vs overlapping; latch inference and simulation-synthesis mismatch.
* **Loops & Generate:** `for` loops for evaluation, `generate` for scalable module instantiation.

**Labs:**



* [**Lab 13: Incomplete If Lab**](./Day5/Lab%20Incomplete%20IF.md)
* [**Lab 14: Incomplete / Overlapping Case Lab**](./Day5/Lab%20Incomplete%20Overlapping%20Case.md)
* [**Lab 15: Loops & Generate Lab**](./Day5/Lab%20Loops.md)


**Observations:**

* Incomplete `if` → latch inferred.
* Complete case → clean mux.
* Incomplete/overlapping case → latch inference & simulation-synthesis mismatch.
* Loops and generate → scalable design, simulation consistent.

---

## Author

**Hirdesh** 
**JSS NOIDA 4th year**
@hirdeshpamani2@gmail.com


---

## Acknowledgment

Special thanks to **Kunal Ghosh** of **VSD** for guidance and support throughout the workshop.

---

