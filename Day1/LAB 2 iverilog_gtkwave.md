## 🧪 Lab 2


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
