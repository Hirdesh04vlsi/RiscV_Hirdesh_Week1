

# 🧪 Lab: Unused Output Optimization Lab

This lab demonstrates **unused output optimization** in Verilog synthesis. Two counters were analyzed:

* `counter_opt` → partial output assigned → only **1 flip-flop synthesized**.
* `counter_opt2` → full output assigned → all **3 flip-flops synthesized**.

We used **Yosys** for synthesis and optimization.

---

## 1️⃣ Counter `counter_opt`

### Yosys Synthesis Commands

```bash
$ yosys << EOF
$ read_liberty ../path.lib
$ read_verilog counter_opt.v
$ synth -top counter_opt
$ dfflibmap -liberty ../path.lib
$ abc -liberty ../path.lib
$ write_verilog -noattr counter_opt_net.v
$ show
$ exit
EOF
```

### Observations

* Only the **used output bit** was synthesized as a D flip-flop.
* Unused bits were **optimized away** automatically.
* The synthesized netlist is smaller and more area/power efficient.

**Schematic & RTL :**

<img width="963" height="455" alt="Screenshot 2025-09-25 003632" src="https://github.com/user-attachments/assets/11fed154-8096-4d9c-8ce5-e2ce994bd1c0" />
<img width="958" height="639" alt="Screenshot 2025-09-25 004055" src="https://github.com/user-attachments/assets/46cfaccc-c895-4d7c-941f-8abb2c504cab" />

---


## 2️⃣ Counter `counter_opt2`

### Yosys Synthesis Commands

```bash
$ yosys << EOF
$ read_liberty ../path.lib
$ read_verilog counter_opt2.v
$ synth -top counter_opt2
$ dfflibmap -liberty ../path.lib
$ abc -liberty ../path.lib
$ write_verilog -noattr counter_opt2_net.v
$ show
$ exit
EOF
```

### Observations

* All **3 output bits** were synthesized as D flip-flops.
* No outputs were removed since all are used.
* Full counter hardware is implemented, reflecting complete RTL functionality.

**Schematic & RTL:**

<img width="605" height="410" alt="Screenshot 2025-09-25 003859" src="https://github.com/user-attachments/assets/8ee6b6b9-4ada-4294-b7f0-b9284b1a9504" />
<img width="1919" height="713" alt="Screenshot 2025-09-25 004551" src="https://github.com/user-attachments/assets/cb67a85a-1c39-45bc-a9f0-c4b3c7532591" />

---

## 3️⃣ Conclusion

* **Unused output optimization** reduces hardware for signals that are not used in the design.
* Partial output assignment results in **fewer flip-flops synthesized** (`counter_opt`).
* Full output assignment ensures **all intended flip-flops are implemented** (`counter_opt2`).
* Synthesis tools like **Yosys** efficiently optimize unused logic while preserving functional outputs.

---
