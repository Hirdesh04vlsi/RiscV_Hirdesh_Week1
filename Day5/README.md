# 📘 VLSI Learning Notes — Day 5

## If/Case Constructs and Loops

---

## 1. **If and If-Else Constructs**

### General Form:

```verilog
always @(posedge clk) begin
    if (condition1)
        statement1;
    else if (condition2)
        statement2;
    else
        statement3;
end
```

### Hardware Mapping:

* Synthesizes into a **MUX**.
* The order of conditions defines **priority** (first true condition is executed).

### Key Points:

* **Priority Logic**: `if-else if` creates *priority encoders*.
* **Missing Else**:

  ```verilog
  if (cond)
      out = 1;
  // missing else → out is not assigned for cond=0 → latch inferred
  ```
* **Incomplete If**: Causes **latches** because output "remembers" its last value when no condition matches.

---

## 2. **Case Construct**

### General Form:

```verilog
always @(*) begin
    case (sel)
        2'b00: out = a;
        2'b01: out = b;
        2'b10: out = c;
        2'b11: out = d;
        default: out = 0;   // recommended to avoid latches
    endcase
end
```

### Hardware Mapping:

* Synthesizes into a **parallel multiplexer**.
* No inherent priority (all options checked simultaneously).

### Key Points:

* **Missing Default**:
  → Latch inferred because not all cases are covered.
* **Overlapping Cases**:

  ```verilog
  case (sel)
    2'b00: out = a;
    2'b0?: out = b;  // overlap → synthesis vs simulation mismatch
  endcase
  ```
* **Partial Assignments** inside a case block also cause latches.

---

## 3. **If vs Case – Difference**

| Aspect                   | If / If-Else                 | Case                                    |
| ------------------------ | ---------------------------- | --------------------------------------- |
| Execution Order          | Sequential (priority logic)  | Parallel (no priority)                  |
| Usage                    | For priority-based decisions | For multi-way choices (decoders, muxes) |
| Default Behavior Needed? | Yes (else to avoid latches)  | Yes (default to avoid latches)          |
| Best Use Case            | Priority encoders            | Multiplexers, decoders                  |

👉 **Both are used only inside `always` blocks**.

---

## 4. **Loops in Verilog**

### (A) For Loop

* **Used inside always blocks** to perform repeated operations (evaluation).

```verilog
integer i;
always @(*) begin
    sum = 0;
    for (i = 0; i < 8; i = i+1) begin
        sum = sum + data[i];  // e.g., summing bits
    end
end
```

➡️ Synthesizer unrolls the loop into combinational logic.

---

### (B) Generate For Loop

* **Used for instantiating multiple hardware blocks**.
* Not allowed inside always blocks.

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i+1) begin : adder_array
        fa u_fa (.a(A[i]), .b(B[i]), .cin(c[i]), .sum(S[i]), .cout(c[i+1]));
    end
endgenerate
```

➡️ Creates **4 instances of full adders** in hardware.

---

### Difference Between For and Generate For

| Feature       | For Loop                   | Generate For Loop             |
| ------------- | -------------------------- | ----------------------------- |
| Context       | Inside always block        | Outside always (structural)   |
| Purpose       | Iterative evaluation       | Hardware instantiation        |
| Example Usage | Accumulation, parity, sums | Ripple-carry adder, bus muxes |

---

# 5. **Labs Performed**

# (A) Lab Incomplete If

* **Files**: `incomp_if.v`, `incomp_if2.v`
* **Observation**: Missing else → Latches inferred in synthesis.

---

### (B) Lab incomplete overlapping Case

* **Files**:

  * `comp_case.v` → Complete case → Synthesizes to clean mux.
  * `incomp_case.v` → Incomplete case → Latches inferred.
  * `partial_case.v` → Partial assignments → Latch inferred.
  * `bad_mux.v` → Overlapping select lines → Simulation vs synthesis mismatch.

---

### (C) Lab Loops

* **Files**:

  * `demux_case.v` → Demux using case.
  * `demux_generate.v` → Demux using generate-for.
  * Both simulated → identical hardware after synthesis.

* **Instantiation Example**:

  * `fa.v` → Full adder module.
  * `rca.v` → Ripple-carry adder built using multiple full adders.
  * `tb_rca.v` → Testbench verifying RCA functionality.

---

✅ **End of Day 5 Learning**

* Learned `if` / `case` constructs, their mapping to hardware, and **pitfalls (latch inference)**.
* Understood **priority vs parallel execution**.
* Explored **for loop vs generate-for loop** with syntax and practical lab examples.
* Practiced instantiation of hierarchical modules (`fa`, `rca`).

---

