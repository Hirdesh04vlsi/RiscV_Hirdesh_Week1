# Sequential Optimization Lab

We analyze **D flip-flops with constant inputs** (`dff_const4` and `dff_const5`) and compare **RTL vs synthesized netlists**. Simulation uses **Icarus Verilog** + **GTKWave**, and synthesis uses **Yosys** with **dfflibmap**.

---

## 1️⃣ DFF_CONST4

### RTL Code

```bash
$ gvim dff_const4.v 
```
<img width="542" height="378" alt="Screenshot 2025-09-25 002444" src="https://github.com/user-attachments/assets/7291d945-d1c8-4c8b-8a2f-4afb5bc02536" />

### Simulate RTL

```bash
$ iverilog dff_const4.out dff_const4.v tb_dff_const4.v
$ ./dff_const4.out
$ gtkwave dump.vcd 
```
 <img width="949" height="1077" alt="Screenshot 2025-09-25 003403" src="https://github.com/user-attachments/assets/5da36af6-5d83-40ea-95f3-a33854f433c7" />

### Synthesize & Optimize

```bash
$ yosys << EOF
$ read_liberty ../path.lib
$ read_verilog dff_const4.v
$ synth -top dff_const4
$ dfflibmap -liberty ../path.lib
$ abc -liberty ../path.lib
$ write_verilog -noattr dff_const4_net.v
$ show
$ exit
EOF
```

### Synthesized Netlist

<img width="604" height="638" alt="Screenshot 2025-09-25 003114" src="https://github.com/user-attachments/assets/b2abe1dc-c8d5-4fa4-9f1a-173fd5ccc92e" />

### Observation

* RTL sets `q` and `q1` to 1 after reset.
* Synthesis simplifies to constant high wires (`1'hl`) since values are fixed.

---

## 2️⃣ DFF_CONST5

### RTL Code

```bash
$ gvim dff_const5.v 
```
<img width="549" height="361" alt="Screenshot 2025-09-25 002431" src="https://github.com/user-attachments/assets/53a83a4b-3202-4074-ad05-bb0d0563d46c" />


### Simulate RTL

```bash
$ iverilog -o dff_const5.out dff_const5.v tb_dff_const5.v
$ ./dff_const5.out
$ gtkwave dump.vcd 
```

<img width="1008" height="796" alt="Screenshot 2025-09-25 003533" src="https://github.com/user-attachments/assets/f000b7b3-8b19-44c5-9fad-e202e73c6a23" />


### Synthesize & Optimize

```bash
$ yosys << EOF
$ read_liberty ../path.lib
$ read_verilog dff_const5.v
$ synth -top dff_const5
$ dfflibmap -liberty ../path.lib
$ abc -liberty ../path.lib
$ write_verilog -noattr dff_const5_net.v
$ show
$ exit
EOF
```

### Synthesized Netlist

<img width="595" height="140" alt="Screenshot 2025-09-26 013932" src="https://github.com/user-attachments/assets/85c5fdfa-c2c3-4026-9bdb-c27bafb31d26" />


### Observation

* RTL resets `q` and `q1` to 0 after reset.
* Synthesis uses actual sequential D flip-flops (`dfrtp`) with proper reset mapping.

---
#Results
---

## 3️⃣ RTL vs Synthesized Comparison

| Module       | RTL Reset Behavior | RTL Post-Clock   | Synthesized Behavior               | Notes                                |
| ------------ | ------------------ | ---------------- | ---------------------------------- | ------------------------------------ |
| `dff_const4` | `q = 1, q1 = 1`    | Always 1         | Constant high wires (`1'hl`)       | Fully optimized; no flip-flop needed |
| `dff_const5` | `q = 0, q1 = 0`    | `q` follows `q1` | Mapped to DFF (`dfrtp`) with reset | Sequential logic preserved           |

---

### RTL vs Netlist Diagram (Conceptual)

```
dff_const4                          dff_const5
+----------+                        +----------+
| RTL      |                        | RTL      |
| clk,reset|                        | clk,reset|
| q,q1=1   |                        | q,q1=0   |
+----------+                        +----------+
      |                                  |
      v                                  v
+------------------+           +---------------------+
| Synthesized Net  |           | Synthesized Net     |
| Constant wires   |           | DFF cells (dfrtp)  |
| q=1,q1=1         |           | q follows q1, reset|
+------------------+           +---------------------+
```

✅ Key Points:

1. **`dff_const4`**: RTL is constant high → synthesis removes flip-flops.
2. **`dff_const5`**: RTL has sequential behavior → synthesis maps to actual DFF cells.
3. **Sequential Optimization**: Tools automatically optimize constants and preserve meaningful sequential logic.
4. **dfflibmap** ensures technology-specific DFF cells are used during synthesis.

---

# Same happens for 
## dff_const.v and dff_const2.v and dff_const3.v

<img width="972" height="1076" alt="Screenshot 2025-09-25 000943" src="https://github.com/user-attachments/assets/a1160be8-4220-4dbe-9c5a-8614ce0e9d0f" />
<img width="1919" height="421" alt="Screenshot 2025-09-25 002005" src="https://github.com/user-attachments/assets/ac7be7c4-8a79-4eec-8937-0464bd55f1b1" />
<img width="958" height="1075" alt="Screenshot 2025-09-25 002109" src="https://github.com/user-attachments/assets/532793d4-1046-4c57-968f-f80f163d2d6f" />
<img width="475" height="360" alt="Screenshot 2025-09-25 002459" src="https://github.com/user-attachments/assets/04ae8ea8-bb97-4261-a6db-537290e438dc" />
<img width="962" height="1079" alt="Screenshot 2025-09-25 003015" src="https://github.com/user-attachments/assets/a5bfc709-6749-4886-adfe-7f70b09a2bea" />

---

## 4️⃣ Conclusion

Sequential optimization removes unnecessary flip-flops for constant signals while retaining critical sequential logic, enabling efficient and correct hardware implementation.

---

