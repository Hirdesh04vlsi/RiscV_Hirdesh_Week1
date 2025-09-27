# Lab Incomplete IF

## Objective

* Understand how **incomplete `if` statements** in Verilog can cause **latches** during synthesis.
* Compare behavior of correct vs incomplete `if` statements in hardware.

## Files

1. `incomp_if.v`
2. `incomp_if2.v`

## Concepts

* An **incomplete `if` statement** is one where **`else` is missing** for some conditions.
* **Synthesis:** Hardware tools infer **latches** to hold values for unspecified conditions.
* **Simulation:** Output may appear correct, but the inferred latches can cause unexpected behavior in hardware.

## incomp_if.v

```bash
$ gvim incomp_if.v
```
<img width="570" height="162" alt="Screenshot 2025-09-27 191330" src="https://github.com/user-attachments/assets/ae6168b2-d210-4aaf-a6fc-3cc0b16628f3" />


```bash
$ yosys
yosys> read_verilog incomp_if.v
yosys> synth -top top_module
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

<img width="955" height="587" alt="Screenshot 2025-09-27 191904" src="https://github.com/user-attachments/assets/65afce65-6108-41e7-99a4-55cd7593bc7f" />

**Observation:** Incomplete `if` → D latch inferred for output.

## incomp_if2.v

```bash
$ gvim incomp_if2.v
$ iverilog incomp_if2.v tb_incomp_if.v
$ ./a.out
$ gtkwave dump.vcd 
```
<img width="587" height="274" alt="Screenshot 2025-09-27 192057" src="https://github.com/user-attachments/assets/e89d24fc-5776-4b2d-b77b-afb2c79b7201" />
<img width="945" height="359" alt="Screenshot 2025-09-27 192414" src="https://github.com/user-attachments/assets/1d9b73de-4aae-4ed4-b35a-f795cc98910a" />


```bash
$ yosys
yosys> read_verilog incomp_if2.v
yosys> synth -top top_module
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
<img width="956" height="413" alt="Screenshot 2025-09-27 193123" src="https://github.com/user-attachments/assets/77a7f950-7403-45da-9405-ce719203bda5" />


**Observation:** Missing `else` → latch inferred.



## Observations from Labs

| File           | Description             | Hardware Effect                  |
| -------------- | ----------------------- | -------------------------------- |
| `incomp_if.v`  | Incomplete `if`         | Latch inferred for output        |
| `incomp_if2.v` | Another incomplete `if` | Latch inferred, similar behavior |

* **Simulation:** Outputs work for some inputs but may retain previous state unexpectedly.
* **Synthesis:** Latches are inferred due to missing `else` paths.

## Conclusion

* Always provide **complete `if-else` paths** to avoid unwanted latches.
* Useful exercise to understand **how conditional statements map to hardware**.
* Demonstrates the difference between **behavioral simulation** and **hardware synthesis**.



---

