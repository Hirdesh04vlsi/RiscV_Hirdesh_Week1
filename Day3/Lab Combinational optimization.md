# 🧪 Lab: Combinational Optimization

## 🎯 Objective

To study **combinational logic optimization** in Yosys and understand how redundant logic is reduced to improve **PPA (Power, Performance, Area)**.

---

## 🧪 Experiment

### Source Files

* `opt_check.v`
* `opt_check2.v`
* `opt_check3.v`
* `opt_check4.v`
* `multiple_modules_opt.v`

### Steps

1. Viewed each file in **GVim** to understand the RTL structure.
2. Ran synthesis in **Yosys** using optimization passes.
3. Used the newly learned **`opt_clean -purge`** command in addition to `opt`.
4. Compared netlists before and after optimization.

```bash
$ gvim opt_check.v opt_check2.v opt_check3.v multiple_modules_opt.v &
$ yosys << EOF
$ read_liberty ../path.lib
$ read_verilog multiple_modules_opt.v
$ synth -top fmultiple_modules_
$ abc -liberty ../path.lib
$ write_verilog -noattr multiple_modules_opt.v
$ flatten
$ opt_clean -purge
$ show
$ exit
EOF
```

## Multiple_modules_opt.v

### 1️⃣ Original RTL (`multiple_modules_opt.v`)
<img width="947" height="437" alt="Screenshot 2025-09-24 235256" src="https://github.com/user-attachments/assets/875770aa-39b9-4627-9639-e042ef620417" />


### Analysis:

1. **sub_module1** → simple AND gate (`y = a & b`).
2. **sub_module2** → XOR gate (`y = a ^ b`).
3. **multiple_module_opt**:

   * Wires: `n1, n2, n3` connect the modules.
   * U1: `n1 = a & 1'b1` → effectively `n1 = a`.
   * U2: `n2 = n1 ^ 1'b0` → XOR with 0 has **no effect**, so `n2 = n1 = a`.
   * U3: `n3 = b ^ d`.
   * Final output: `y = c | (b & n1)` → `y = c | (b & a)`.

✅ Notice how **some gates and XORs are redundant**. For example, `n2` is just equal to `a`.

---
### 2️⃣ Synthesized & Flattened Netlist (`multiple_modules_flat.v`)
<img width="1231" height="808" alt="Screenshot 2025-09-25 000545" src="https://github.com/user-attachments/assets/faaa6865-abbd-4fe9-998f-e1a0a90a37c5" />
<img width="608" height="184" alt="Screenshot 2025-09-25 000608" src="https://github.com/user-attachments/assets/b471e5e7-9e18-4631-8c18-683c2d68523d" />


### Observations:

1. **Flattening**:

   * All modules are merged into a single module.
   * Module hierarchy removed.

2. **Redundant logic removed**:

   * U2 disappeared because `n2 = n1 ^ 0 = n1` → no separate logic needed.
   * Only **essential wires remain** (`n1`, `_0_`).

3. **Optimized expressions**:

   * `_0_ = n1 & b` → corresponds to `b & a`.
   * `y = _0_ | c` → corresponds to `c | (b & a)` (final simplified output).

4. **Internal wires like `\U1.a`, `\U1.b`** are retained for connection clarity, but the extra XOR/AND gates were simplified during **`opt` and `opt_clean -purge`**.

---



# Same goes on for these 4 codes too
## opt_check.v
<img width="908" height="105" alt="Screenshot 2025-09-24 235217" src="https://github.com/user-attachments/assets/8cb79cd6-9317-4d43-a0f6-831f0e644eff" />
<img width="781" height="1012" alt="Screenshot 2025-09-24 230053" src="https://github.com/user-attachments/assets/f6bcf91d-b11c-4c2e-9c10-b392f7f35e9d" />

## opt_check2.v
<img width="963" height="119" alt="Screenshot 2025-09-24 235236" src="https://github.com/user-attachments/assets/8b83d28f-fa0c-470a-8405-8042994ef8a9" />
<img width="956" height="1079" alt="Screenshot 2025-09-24 230307" src="https://github.com/user-attachments/assets/23bfff59-59c4-45b9-862e-05a1fb9333e9" />

## opt_check3.v
<img width="875" height="137" alt="Screenshot 2025-09-24 235243" src="https://github.com/user-attachments/assets/6056eab0-f5cf-4e82-b5fa-9661ca938ea4" />
<img width="963" height="1079" alt="Screenshot 2025-09-24 230549" src="https://github.com/user-attachments/assets/b37085a5-4700-4f6a-8e3e-16e225550c35" />

## opt_check4.v
<img width="866" height="108" alt="Screenshot 2025-09-24 235249" src="https://github.com/user-attachments/assets/0c46a36b-3158-4719-a03f-6ae06b681eeb" />
<img width="627" height="273" alt="Screenshot 2025-09-24 235537" src="https://github.com/user-attachments/assets/0d293270-38d1-4c22-b6a7-aecb2214490f" />


## 🔎 Observations

* Many signals and gates were removed as they were redundant.
* Circuits that initially appeared larger collapsed into much smaller equivalent logic.
* Example: nested MUX design was simplified into a single **XNOR gate**.
* Using `opt_clean -purge` gave an even **cleaner netlist**, stripping away unused wires and cells.

---

## ✅ Conclusion

* Combinational optimization significantly reduces logic complexity.
* **PPA benefits**:

  * Fewer gates → **lower power**.
  * Shorter paths → **better performance**.
  * Smaller design → **less area**.
* The command **`opt_clean -purge`** is very powerful in ensuring all unused logic is eliminated.

---

