

# 🧪 Lab: Hierarchical vs Flat Synthesis

This lab demonstrates how Yosys handles synthesis when preserving **hierarchy** vs when **flattening** the design.  
We use the file `multiple_modules.v` for this experiment.

---

## 1️⃣ Prepare the Verilog File
```bash
$ gvim multiple_modules.v
````


---

## 2️⃣ Hierarchical Synthesis Flow

```bash
$ yosys
yosys> read_liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> abc -liberty ../my_lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr multiple_modules_hier.v
yosys> !gvim multiple_modules_hier.v
```

✅ **Observation:**

* Netlist still shows submodules preserved.
* Easier to debug and reuse.
<img width="959" height="1079" alt="Screenshot 2025-09-24 011442" src="https://github.com/user-attachments/assets/905fc3b9-5f09-4a4a-81b9-a819857c4e2b" />
<img width="950" height="644" alt="Screenshot 2025-09-24 011745" src="https://github.com/user-attachments/assets/4c6b080a-f601-4845-843d-c0ff3d9b950e" />
<img width="492" height="999" alt="Screenshot 2025-09-24 012138" src="https://github.com/user-attachments/assets/f1ed2842-9459-4ef7-8a7d-4a0cd31360ee" />

---


---

## 3️⃣ Flattened Synthesis Flow

```bash
$ yosys
yosys> read_liberty ../my_lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> flatten
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr multiple_modules_flat.v
yosys> !gvim multiple_modules_flat.v
```


✅ **Observation:**

* All hierarchy is removed.
* Final netlist is a flat gate-level representation.
* Tool may optimize across boundaries → potentially better QoR (Quality of Results).

<img width="1908" height="199" alt="Screenshot 2025-09-24 013245" src="https://github.com/user-attachments/assets/abeb4d26-9557-437a-86b7-3570493f5953" />

<img width="778" height="1079" alt="Screenshot 2025-09-24 013049" src="https://github.com/user-attachments/assets/b15113a6-151f-4ce8-ae91-2aa6cf3c9787" />

---



---

## 4️⃣ Submodule Synthesis

Sometimes, synthesizing **only a submodule** is useful, especially in large SoCs where submodules are instantiated multiple times.

### Example: synthesize submodule `sub_module1`

```bash
$ yosys
yosys> read_liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top sub_module1
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr sub_module1_synth.v
yosys> !gvim sub_module1_synth.v
```


✅ **Observation:**

* Only the logic for `sub_module1` is synthesized.
* This approach is valuable for **debugging**, **IP block development**, or **designs with repeated submodules**.

<img width="953" height="582" alt="Screenshot 2025-09-24 013713" src="https://github.com/user-attachments/assets/97db5cbc-ced4-4066-8514-0c8df7e4b536" />
<img width="606" height="159" alt="Screenshot 2025-09-24 013746" src="https://github.com/user-attachments/assets/836478e5-6b08-4b79-a0f4-6c648ce15f3c" />
---

## 📂 Outputs

* `multiple_modules_hier.v` → Hierarchical netlist
* `multiple_modules_flat.v` → Flattened netlist
* `sub_module1_synth.v` → Synthesized submodule

```
