
# 🧪 Lab: Multiplication Optimizations (mult_2 and mult_9)

This lab explores how synthesis tools like **Yosys** optimize multiplications by constants.  
We demonstrate with two designs: `mult_2.v` and `mult_8.v` (which actually implements multiply by 9 using concatenation).

---

## 1️⃣ Theory Recap

- **Multiplication by powers of 2**  
  Can be reduced to **bit-shifts**:  
  - `a * 2 = a << 1` → `{a, 0}`  
  - `a * 8 = a << 3` → `{a, 000}`  

- **Multiplication by 9**  
  Implemented in code as concatenation `{a,a}`:  
  - `{a,a} = (a << 3) + a`  
  - So, effectively → `a * 9`  

✅ Such optimizations save **area and power** because no multiplier hardware is needed.

---

```bash
$ gvim mult_*.v -o 
```
<img width="968" height="1075" alt="Screenshot 2025-09-24 040359" src="https://github.com/user-attachments/assets/d894a13a-78c0-45ff-8392-e8b850ddb621" />



---

 
## 3️⃣ Synthesis with Yosys

### Multiply by 2
```bash
$ yosys
yosys> read_liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog mult_2.v
yosys> synth -top mult_2
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr mult_2_netlist.v
yosys> show
```
<img width="1919" height="1077" alt="Screenshot 2025-09-24 040823" src="https://github.com/user-attachments/assets/43ff9a3f-da66-417c-a578-cc3b1c5997b9" />

<img width="553" height="304" alt="Screenshot 2025-09-24 041319" src="https://github.com/user-attachments/assets/8ae67f5f-1c0f-4e3e-a6f3-764d43b53542" />

---

### Multiply by 9

```bash
yosys> read_liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog mult_8.v
yosys> synth -top mult_8
yosys> abc -liberty ./sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr mult_8_netlist.v
yosys> show
```


<img width="964" height="1076" alt="Screenshot 2025-09-24 041449" src="https://github.com/user-attachments/assets/6cf8c0a4-2b5e-4163-af1e-a0223aebe94d" />

<img width="948" height="1079" alt="Screenshot 2025-09-24 041528" src="https://github.com/user-attachments/assets/c07bc6f7-4fa0-41f5-9bb6-fdbc9087fe02" />

---

## 4️⃣ Results & Observations

* **mult\_2**
  Optimized to wiring `{a, 1'b0}`.

* **mult\_9 (mult\_8.v)**
  Optimized to shift + add: `(a << 3) + a`.
  <img width="941" height="449" alt="Screenshot 2025-09-24 041431" src="https://github.com/user-attachments/assets/f14e9983-4a67-4c9b-a156-b4f4279ba187" />


* **General insight:**

  * Synthesis tools exploit arithmetic identities.
  * This avoids heavy multiplier logic.
  * Saves **area**, **power**, and improves **performance**.

---

## 📂 Outputs

* Synthesized netlists: `mult_2_netlist.v`, `mult_8_netlist.v`

---

