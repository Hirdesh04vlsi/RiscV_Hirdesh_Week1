## 🧪 Lab 3

```bash
$ yosys
yosys> read_liberty -lib ../path/lib
yosys> read_verilog good_mux.v
yosys> synth -top good_mux
yosys> abc -liberty ../path/lib
yosys> show
yosys> write_verilog good_mux.netlist.v
yosys> !gvim good_mux.netlist.v
yosys> write_verilog -noattr good_mux_netlist.v
yosys> !gvim good_mux_netlist.v
yosys> exit
```


```bash
yosys
```
<img width="1060" height="609" alt="Screenshot 2025-09-19 013924" src="https://github.com/user-attachments/assets/a1e03136-f1db-417e-b921-be0493e08a41" />

Enter yosys shell.

```bash
read_liberty -lib ../path/lib
```
<img width="714" height="230" alt="Screenshot 2025-09-22 001003" src="https://github.com/user-attachments/assets/e6a3300b-f56d-4029-8457-6509b0b746cf" />

Read standard cell library.

```bash
read_verilog good_mux.v
```

Load RTL design.

```bash
synth -top good_mux
```
<img width="961" height="639" alt="Screenshot 2025-09-22 001107" src="https://github.com/user-attachments/assets/17988928-2f0f-4b5b-a7f0-4584699aeb89" />

Run synthesis with `good_mux` as top module.

```bash
abc -liberty ../path/lib
```

Technology mapping: assign logic to actual cells.

```bash
show
```
<img width="958" height="1079" alt="Screenshot 2025-09-22 001321" src="https://github.com/user-attachments/assets/82f64aa0-34a1-4877-aa7a-5a2f5339108a" />
Visual schematic of synthesized design.

```bash
write_verilog good_mux.netlist.v
```

Write synthesized netlist (with attributes).

```bash
!gvim good_mux_netlist.v
```
<img width="1093" height="1079" alt="Screenshot 2025-09-22 002957" src="https://github.com/user-attachments/assets/201d479a-bcf6-4d31-a713-07c729df4a7d" />

Open netlist in `gvim`.

```bash
write_verilog -noattr good_mux_netlist.v
```

Write netlist without synthesis attributes.

```bash
!gvim good_mux_netlist.v
```
<img width="757" height="1079" alt="Screenshot 2025-09-22 003350" src="https://github.com/user-attachments/assets/979c90c2-ec94-4b29-b6e5-1abf9f747e6a" />

View clean netlist.

---
