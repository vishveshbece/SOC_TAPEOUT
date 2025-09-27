# Day 3: Combinational and Sequential Optimizations

This document covers the fundamental concepts of logic optimization in digital design, breaking them down into combinational and sequential techniques. Optimization is a crucial step in the synthesis process to meet design goals for area, speed, and power.

---

## 1. Introduction to Optimizations

### What is Logic Optimization?

Logic optimization is the process of transforming a digital circuit's logic representation (like a Boolean expression or a netlist) into a more efficient one while preserving its functionality. This is a key step performed by synthesis tools like Yosys.

### The Goals of Optimization

The primary goals, often referred to as **PPA (Power, Performance, and Area)**, are:

1.  **Minimize Area:** Reduce the physical size of the circuit on the silicon chip. This is achieved by using fewer logic gates and registers, which lowers manufacturing costs.
2.  **Maximize Performance (Speed):** Decrease the propagation delay through the circuit's critical path. This allows the circuit to run at a higher clock frequency.
3.  **Minimize Power Consumption:** Reduce the energy consumed by the circuit. This is critical for battery-powered devices and for managing heat in high-performance systems.

These goals often involve trade-offs. For example, optimizing for maximum speed might require adding more logic, which increases the area and power consumption.

---

## 2. Combinational Logic Optimizations

Combinational logic optimizations focus on circuits without memory elements (like flip-flops or latches). The output depends solely on the current inputs.

### Key Techniques

#### a. Boolean Logic Simplification

This is the process of reducing complex Boolean expressions to their simplest forms using algebraic rules.

* **Algebraic Rules:** Applying identities like `$A + AB = A$`, `$A \cdot (A+B) = A$`, and De Morgan's laws.
* **Karnaugh Maps (K-maps):** A graphical method used to simplify expressions with a small number of variables.
* **Quine-McCluskey Algorithm:** A tabular method that is more systematic and can be automated for expressions with many variables.

**Example:**
The expression `$F = A\bar{B}C + ABC + \bar{A}BC$` can be simplified.
* Factor out `AC`: `$F = AC(\bar{B} + B) + \bar{A}BC$`
* Since `$\bar{B} + B = 1$`: `$F = AC + \bar{A}BC$`
* Factor out `C`: `$F = C(A + \bar{A}B)$`
* Using the adjacency rule `$A + \bar{A}B = A+B$`: `$F = C(A+B)$`

#### b. Constant Propagation

Synthesis tools identify signals that are tied to a constant value (logic `0` or `1`) and propagate this value through the logic, simplifying or eliminating gates.

* An `AND` gate with one input tied to `0` will always output `0`.
* An `OR` gate with one input tied to `1` will always output `1`.
* An `XOR` gate with one input tied to `0` becomes a buffer.

#### c. Common Subexpression Elimination

The tool identifies identical logic structures (subexpressions) that appear multiple times in the design and implements them only once. The single output is then shared among all the different places it's needed, reducing the total gate count.

**Example:**
If you have:
`x = (a & b) | c;`
`y = (a & b) & d;`

The tool will synthesize the `(a & b)` logic once and share its output.

---

## 3. Sequential Logic Optimizations

Sequential logic optimizations involve circuits with state-holding elements like flip-flops. These optimizations consider the temporal behavior of the circuit.

### Key Techniques

#### a. State Machine Optimization

For Finite State Machines (FSMs), optimization can be done at two levels:

* **State Minimization:** Removing redundant or equivalent states from an FSM to reduce the total number of states. A smaller state machine requires fewer flip-flops for state representation and can result in simpler next-state logic.
* **State Encoding:** The way states are represented in binary values affects the complexity of the combinational logic. Common encoding schemes include:
    * **Binary Encoding:** Uses the minimum number of flip-flops ($log_2 N$ for N states).
    * **One-Hot Encoding:** Uses N flip-flops for N states, where only one flip-flop is active (`1`) at any time. This often leads to simpler, faster logic at the cost of more flip-flops.
    * **Gray Encoding:** Ensures that only one bit changes between adjacent states, which can help reduce glitches and power consumption.

#### b. Retiming

Retiming is a powerful technique that repositions registers (flip-flops) within the circuit without changing its functional behavior. The goal is to balance the combinational logic delay between registers. By shortening the longest logic path (the critical path), retiming allows the circuit to be clocked at a higher frequency.

* **Forward Retiming:** Moving a register from the inputs of a logic gate to its output.
* **Backward Retiming:** Moving a register from the output of a logic gate to its inputs.

#### c. Clock Gating

This is a major power-saving technique. Instead of letting the clock signal run continuously to all flip-flops, the clock to a block of registers is temporarily turned off (gated) when the registers are not changing their values. This prevents unnecessary switching activity in the registers and downstream logic, significantly reducing dynamic power consumption.

---

## 4. Sequential Optimizations for Unused Outputs

This is a specific form of optimization related to removing "dead code" from a design.

### Logic Trimming and Removal

During synthesis, the tool analyzes the connectivity of the entire design. If it finds a register whose output (`Q` pin) is not connected to any other part of the circuit (i.e., it's an **unused output** or has no fan-out), it determines that the register serves no purpose.

The optimization process is as follows:
1.  **Identify Unused Register:** The synthesis tool flags any register whose state is never used.
2.  **Trace Back Logic:** It then traces back to the combinational logic that computes the input (`D` pin) for that register.
3.  **Remove Logic:** The unused register and any combinational logic that *exclusively* feeds it (and has no other fan-out) are completely removed from the netlist.

This is highly effective for cleaning up designs, removing old or debug logic, and reducing both area and power consumption.

**Example in Verilog:**

```verilog
module top (
    input clk,
    input a,
    input b,
    output logic y
);

    logic temp_val;
    logic unused_val;

    // This register's output is used
    always @(posedge clk) begin
        temp_val <= a & b;
    end

    // This register's output is NOT used anywhere
    always @(posedge clk) begin
        unused_val <= a | b;
    end

    assign y = ~temp_val;

endmodule