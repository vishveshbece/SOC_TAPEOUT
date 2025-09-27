# Week 1, Day 1: Verilog Simulation and Logic Synthesis

Welcome to the first day of our workshop! Today, we will dive into the fundamentals of digital design using open-source tools. We'll start with writing and simulating Verilog code and then move on to synthesizing our design into a gate-level netlist using the SKY130 Process Design Kit (PDK).

---

## 1. Introduction to Open-Source Verilog Simulators

**What is Verilog?**
Verilog is a Hardware Description Language (HDL) used to model electronic systems. It allows us to describe a digital circuit's design and behavior, which can then be tested (simulated) and built (synthesized).

**Our Toolchain:**
For this lab, we will use two primary open-source tools for simulation:
* **Icarus Verilog (`iverilog`):** This is a Verilog compiler and simulator. It takes our Verilog files as input, compiles them into an executable format, and runs the simulation.
* **GTKWave:** This is a waveform viewer. After running a simulation, `iverilog` generates a waveform dump file (usually a `.vcd` file). GTKWave allows us to open this file and visually inspect the signals in our circuit over time, which is crucial for debugging.

The typical simulation flow looks like this:

1.  **Write Code:** Create your Verilog design file (`.v`) and a testbench file (`_tb.v`).
2.  **Compile:** Use `iverilog` to compile both files into an executable.
3.  **Simulate:** Run the compiled file to generate the waveform data (`.vcd`).
4.  **Analyze:** Open the `.vcd` file with `GTKWave` to view and analyze the results.

***
👇 **ADD YOUR SCREENSHOT HERE** 👇

*Replace the path below with a screenshot of the GTKWave interface or a diagram of the simulation flow.*

![Simulation Flow Diagram](path/to/your/simulation_flow_diagram.png)
***

---

## 2. Lab 1: Simulation using `iverilog` and `GTKWave`

In this lab, we will simulate a simple 4-bit ripple counter.

### Step 1: Verilog Code
Create a file named `ripple_counter.v` and add the following code:

```verilog
// 4-bit ripple counter
module ripple_counter(
    input clk,
    input reset,
    output [3:0] q
);

    // T-FlipFlop module
    T_FF tff0(clk, reset, q[0]);
    T_FF tff1(q[0], reset, q[1]);
    T_FF tff2(q[1], reset, q[2]);
    T_FF tff3(q[2], reset, q[3]);

endmodule

// T-FlipFlop behavioral model
module T_FF(
    input clk,
    input reset,
    output reg q
);

    always @(posedge clk or posedge reset) begin
        if (reset)
            q <= 1'b0;
        else
            q <= ~q;
    end
endmodule