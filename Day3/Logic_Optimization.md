
# 📘 VLSI Learning Notes — Day 3

## 🔹 Logic Optimization

**Logic Optimization** is the process of transforming a digital circuit into a more efficient version without changing its functionality.
It is mainly performed during **synthesis** to improve **PPA (Power, Performance, Area)**.

* **Power** → Reducing redundant logic lowers switching activity.
* **Performance** → Shorter logic paths improve timing.
* **Area** → Unused gates and flip-flops are eliminated, saving silicon space.

---

## 🔹 New Yosys Commands Learned

* **`opt_clean -purge`** → Aggressive cleanup; also removes internal `$*_` structures not required in final netlist.

These commands are essential to produce minimal, optimized netlists.

---

# 🧪 [Lab: Combinational Optimization](./Lab%20Combinational%20optimization.md)

### Theory

**Combinational logic optimization** focuses on simplifying purely logic-based circuits without memory.

**Types:**

1. **Constant Propagation** → Replace signals driven by 0/1 directly. Example: `y = a & 1` → `y = a`.
2. **Boolean Simplification** → Reduce expressions via algebraic rules. Example: `y = a | (a & b)` → `y = a`.
3. **Mux Restructuring** → Flatten nested multiplexers. Example:

   ```
   y = a ? (b ? c : (c ? a : 0)) : (!c)
   ```

   Naive → 3 MUXes.
   Optimized → `y = a ⊙ c` (XNOR).

**File used:** `multiple_modules_opt.v`

* Showed how different modules collapsed into smaller equivalent logic after optimization.

---

# 🧪 [Lab: Sequential Optimization](./Lab%20Sequential%20optimization.md)

### Theory

**Sequential logic optimization** deals with circuits that contain memory (flip-flops, registers, FSMs).

**Types:**

1. **Sequential Constant Propagation** → Removes flip-flops with constant inputs.
2. **State Optimization** → Eliminates unreachable states in FSMs.
3. **Retiming** → Moves flip-flops across logic to balance timing paths.
4. **Sequential Cloning (Floorplan-Aware Synthesis)** → Duplicates registers to reduce fanout and help placement.

**Files used:**

* `dff_const1.v`
* `dff_const2.v`
* `dff_const3.v`
* `dff_const4.v`
* `dff_const5.v`

Observation: Though coded differently, synthesis produced equivalent, smaller optimized circuits.

---

# 🧪 [Lab: Unused Output Optimization](./Lab%20Unused%20output%20optimization.md)

### Theory

When only part of the output is required, synthesis automatically removes unused flip-flops and logic.

**Files used:**

* `counter_opt.v` → Only `q[0]` used → circuit reduced to **1 FF**.
* `counter_opt2.v` → Full `q[2:0]` used → circuit retained **3 FFs**.

---

## 📂 Folder Structure (Day 3)

```
Day3/
 ├── Lab Combinational optimization.md
 ├── Lab Sequential optimization.md
 ├── Lab Unused output optimization.md
 └── Logic_Optimization.md
```
