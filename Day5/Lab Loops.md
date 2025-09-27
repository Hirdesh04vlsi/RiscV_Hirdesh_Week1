
# Loops Lab

## 🔬 Lab Files

* `demux_case.v`
* `demux_generate.v`
* `fa.v`
* `rca.v`
* `tb_rca.v`

---

## 🖥️ Lab Work

### (A) Demux using Case vs Generate

```bash
# View RTL files
gvim demux_case.v
gvim demux_generate.v
```

<img width="642" height="547" alt="Screenshot 2025-09-27 202301" src="https://github.com/user-attachments/assets/a4ff4007-cc94-4416-a3d3-da4101cc1b67" />
<img width="639" height="436" alt="Screenshot 2025-09-27 202354" src="https://github.com/user-attachments/assets/65732bd6-ca41-47b4-854b-3295e9bfc354" />



```bash
# Simulate with Icarus Verilog
iverilog demux_case.v tb_demux.v
./a.out
gtkwave dump.vcd 
```

<img width="1019" height="682" alt="Screenshot 2025-09-27 223100" src="https://github.com/user-attachments/assets/50193bf2-a1cc-47e1-abb3-ad4c522be6f4" />


```bash
# Simulate with Icarus Verilog
iverilog demux_generate.v tb_demux.v
./a.out
gtkwave dump.vcd 
```

<img width="998" height="683" alt="Screenshot 2025-09-27 223225" src="https://github.com/user-attachments/assets/0ed5426f-b3df-409e-b6b3-715ebc00c9e0" />


**Observation:**


* Both versions (`case` and `generate-for`) synthesized to **identical hardware**.
* Waveforms matched → functionality equivalent.

---

### (B) Ripple-Carry Adder using Generate

```bash
# View RTL and testbench
gvim fa.v
gvim rca.v
```
<img width="1285" height="691" alt="Screenshot 2025-09-27 230544" src="https://github.com/user-attachments/assets/636fb500-8c4c-419e-82e4-514fa4b33ca3" />

```bash
# Simulate with Icarus Verilog
iverilog fa.v rca.v tb_rca.v
./a.out
gtkwave dump.vcd 
```

<img width="1001" height="505" alt="Screenshot 2025-09-27 223750" src="https://github.com/user-attachments/assets/a0e48dd6-0068-4dd6-bd0f-5eb8d76628bd" />


**Observation:**

* RCA produced correct **Sum** outputs.
* Using `generate-for` simplified the instantiation of multiple Full Adders.

---

## ✅ Final Notes

* **Demux (case vs generate):** Hardware matched in both implementations.
* **RCA with generate:** Clean and scalable design, simulation successful.

---
