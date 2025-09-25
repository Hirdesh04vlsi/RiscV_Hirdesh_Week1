
# 📘 VLSI Learning Notes — Day 4

## Gate-Level Simulation (GLS)

**What is GLS:**

* GLS is the simulation of a **synthesized netlist**, i.e., the circuit after synthesis mapped to standard cells.
* Unlike RTL simulation (behavioral Verilog), GLS reflects **actual gate-level behavior** and **delays**.

**Why GLS is needed:**

1. **Check logical correctness after synthesis** – ensure the netlist behaves like RTL.
2. **Detect synthesis-simulation mismatches** – issues from blocking/non-blocking assignments or missing sensitivity lists.
3. **Timing validation** – can simulate realistic gate timing.

**How GLS is done:**

1. Synthesize the RTL with **Yosys** to generate a netlist.
2. Simulate the **same testbench**, using the netlist as DUT.
3. Observe waveforms in **GTKWave**.


```bash
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v file.v tb_file.v
$ ./a.out
$ gtkwave dump.vcd
```
---

## Synthesis-Simulation Mismatch

**What it is:**

* When **RTL simulation** and **GLS simulation** produce different results.
* Common causes:

  1. Missing sensitivity lists in combinational always blocks.
  2. Improper use of blocking (`=`) vs non-blocking (`<=`) assignments.
  3. Non-standard Verilog coding.

**Key Concepts:**

### Missing Sensitivity List

* If an always block does not include all signals that affect outputs, it can behave like a flip-flop instead of combinational logic.
* Safe approach: use `always @(*)` to automatically include all RHS signals.

### Blocking vs Non-Blocking Assignments

| Aspect        | Blocking (`=`)                                 | Non-blocking (`<=`)         |
| ------------- | ---------------------------------------------- | --------------------------- |
| Execution     | Immediate, sequential                          | Scheduled for next timestep |
| Typical use   | Combinational logic                            | Sequential/FF logic         |
| Mismatch risk | Can cause GLS-RTL mismatch in sequential logic | Safer in sequential logic   |

---

# 🧪 Labs — Day 4

## **[Lab GLS](./Lab%20GLS(gate%20level%20simulaton.md)**

**What was done:**

* Ran **gate-level simulation** on the synthesized netlist of `ternary_operator.v`.
* Verified that the GLS output matched RTL simulation, confirming **logical correctness**.

---

## **[Lab Synthesis Simulation Mismatch (Missing Sensitivity List)](./Lab%20Synthesis%20Simulation%20Mismatch%20(Missing%20Sensitivity%20List.md)**

**What was done:**

* Investigated **synthesis-GLS mismatch** due to missing signals in the always block (`bad_mux.v`).
* Observed combinational logic behaving like flip-flops.
* Learned importance of using **`always @(*)`**.

---

## **[Lab Synthesis Simulation Mismatch (Blocking/Non-Blocking Statements)](./Lab%20Synthesis%20Simulation%20Mismatch%20(Blocking_Non-Blocking%20Statements.md)**

**What was done:**

* Explored mismatch caused by incorrect **blocking vs non-blocking assignments** (`blocking_caveat.v`).
* Observed netlist simulation did not match RTL when blocking assignments were used in sequential logic.
* Learned proper assignment style to prevent mismatch.

---

## Summary of Day 4

1. GLS ensures post-synthesis logical correctness.
2. Missing sensitivity lists can make combinational logic behave like FFs.
3. Blocking/non-blocking assignments must be used carefully in sequential blocks.
4. Labs covered:

   * GLS (`ternary_operator.v`)
   * Missing sensitivity (`bad_mux.v`)
   * Blocking/non-blocking mismatch (`blocking_caveat.v`)

---


