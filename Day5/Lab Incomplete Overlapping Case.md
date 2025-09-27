
# Lab Incomplete Overlapping Case

## Files

* `comp_case.v` – complete case (pure mux)
* `incomp_case.v` – incomplete case (latch inferred)
* `partial_case.v` – partial overlapping case (simulation–synthesis mismatch)
* `bad_case.v` – bad case (mux + latch + mismatch)

---

## Complete Case

```bash
$ gvim comp_case.v
$ iverilog comp_case.v tb_case.v
$ ./a.out
$ gtkwave dump.vcd 
```
<img width="664" height="228" alt="image" src="https://github.com/user-attachments/assets/c4be7992-5e35-4668-9da9-007e82363012" />
<img width="998" height="522" alt="Screenshot 2025-09-27 194710" src="https://github.com/user-attachments/assets/3792fd9d-84d5-41e3-98cb-76f7d9247744" />


```bash
$ yosys
yosys> read_verilog comp_case.v
yosys> synth -top comp_case
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

<img width="941" height="408" alt="Screenshot 2025-09-27 194531" src="https://github.com/user-attachments/assets/243750e2-e546-46d5-925a-60db0fd4c080" />

**Observation:** Clean mux, no latches.

---

## Incomplete Case

```bash
$ gvim incomp_case.v
$ iverilog incomp_case.v tb_case.v
$ ./a.out
$ gtkwave dump.vcd 
```
<img width="668" height="364" alt="Screenshot 2025-09-27 193732" src="https://github.com/user-attachments/assets/68542098-a82a-45aa-8917-2370b801a2d1" />


```
$ yosys
yosys> read_verilog incomp_case.v
yosys> synth -top incomp_case
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
<img width="997" height="522" alt="Screenshot 2025-09-27 193956" src="https://github.com/user-attachments/assets/08d3735b-9289-4332-84cc-77e05743b79b" />
<img width="969" height="307" alt="Screenshot 2025-09-27 194103" src="https://github.com/user-attachments/assets/84d3ce59-3d79-4291-9f23-c8f06ffc9eee" />

**Observation:** Missing default → latch inferred.


---

## Partial Case

```bash
$ gvim partial_case.v
$ iverilog partial_case.v tb_case.v
$ ./a.out
$ gtkwave dump.vcd 
```

<img width="639" height="406" alt="Screenshot 2025-09-27 194826" src="https://github.com/user-attachments/assets/f0c64a02-b1a3-4c73-a449-0f7c119b6d92" />

```bash
$ yosys
yosys> read_verilog partial_case.v
yosys> synth -top partial_case
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
<img width="971" height="469" alt="Screenshot 2025-09-27 195231" src="https://github.com/user-attachments/assets/a1e055f4-b89c-484d-8353-20d38bfa5027" />

**Observation:** Partial Case statements causing Y to get MUX while X gets a infererd D Latch.

### Boolean expressions (from netlist):

Let `sel = {s1, s0}`

* **Latch enable:**

  ```
  en = s1 ∨ ¬s0
  
  ```

* **d (latch data input):**

  ```
  d = (¬s1 ∧ i2) ∨ (s1 ∧ i1)
  ```
* **y (combinational output):**

  ```
  y = (s1 ∧ i2) ∨ (¬s1 ∧ ¬s0 ∧ i0) ∨ (¬s1 ∧ s0 ∧ i1)
  ```

---

## Bad Case

```bash
$ gvim bad_case.v
$ iverilog bad_case.v tb_case.v
$ ./a.out
$ gtkwave dump.vcd 
```
<img width="629" height="290" alt="Screenshot 2025-09-27 194224" src="https://github.com/user-attachments/assets/5ad65b26-4074-4270-b288-df8128590e27" />
<img width="1033" height="695" alt="Screenshot 2025-09-27 194339" src="https://github.com/user-attachments/assets/67ea90d4-3119-4f32-95c1-3f318ce5708a" />


```bash
$ yosys
yosys> read_verilog bad_case.v
yosys> synth -top bad_case
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
<img width="957" height="695" alt="Screenshot 2025-09-27 194443" src="https://github.com/user-attachments/assets/e4e71c49-6ba9-41e6-ae71-6b9f1dbf8d60" />


```bash
# GLS
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
$ ./a.out
$ gtkwave tb_bad_mux.vcd
```

<img width="991" height="550" alt="Screenshot 2025-09-27 200352" src="https://github.com/user-attachments/assets/b21e1363-e837-4828-be8e-0b74cea48ec8" />


**Observation:** Bad coding → Synthesis Simulation Mismatch

---
