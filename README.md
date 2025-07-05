# 🔐 Encryption-Decryption-25


#  The Key-Controlled Bitstream Puzzle

Welcome to **CRYPTOVERIL++**, a hands-on digital hardware challenge where you won’t just write code — you’ll need to trace signals, analyze waveforms, and uncover how a stream of data is transformed through a mysterious pipeline.

This isn’t your average Verilog assignment. You’ll be designing a complete system from scratch that takes a bitstream and a control key, pushes it through a custom-built multi-stage logic system, and outputs a transformed version. You’ll also need to reason about clock domains, buffers, and how data moves over time — just like in real-world hardware systems.

# Announcement !!

Team submisssions can be of sizes two - three members !

---

## 🔹 What is This Challenge About?

You are given two things:

* A 16-bit input called `input_data`
* A compact control key called `key_bits`

These two values are fed into a three-stage hardware system. The input gets processed in three steps, with each stage performing a transformation based on specific bits from the key. Your task is to build this system, one stage at a time, while understanding how the control logic and data transformation interact.

You will:

* Design each stage as a separate module
* Use a different clock for each stage (each slower than the previous one)
* Add buffering between the stages
* Use Verilator and GTKWave to simulate and observe the signals
* Verify that your design works by matching outputs

---

## 🔎 What's the Goal?

Create a Verilog-based hardware pipeline that:

1. Takes in a 16-bit input
2. Uses a key to control how that input is transformed
3. Passes the data through three different stages
4. Outputs the transformed data after the final stage
5. Can be verified through waveform analysis

You must write all three stages yourself. The only information you have is:

* The format of the key
* The architectural rules
* A hint about what each stage does (but not exact logic)

---

## Verilator GTK wave installation

check out this link ! : https://github.com/SCC-IIITDM/tools

---
## 🌐 System Overview

This system consists of three major logic stages:



### Stage 1: Bit Manipulation Stage

* Operates on the **fastest clock**
* Reads the **first 3 bits of the key** (consider from most significant bit)
* Transforms the input using **bit shifts** and **arithmetic**

### Stage 2: Decision Tree Stage

* Operates on a **clock that is 1/3rd the speed of Stage 1**
* Reads the **next 2 bits of the key**.
* Uses a **4-branch decision tree** to determine how to transform the data further.

### Stage 3: Post-Processing Stage

* Operates on a **clock that is 1/3rd the speed of Stage 2**
* Depending on the decision tree's result, it manipulates the added bits to perform the final cleanup.

---

## ⏱ Clocking Architecture

Each stage runs at a different clock rate:

* Stage 1: `clk1` (fastest)
* Stage 2: `clk2` = `clk1 / 3`
* Stage 3: `clk3` = `clk2 / 3`

This simulates real-world multi-clock domain systems. Data flows between stages asynchronously, meaning you’ll need to synchronize and buffer data properly.

---

## ⛽ Pipelining & Buffers

Because each stage runs at a different speed, you can’t just pass data from one to the next without problems. You need **buffers** between the stages to hold the output of the faster stage until the slower stage is ready.

Each buffer must:

* Store the output of the current stage
* Wait for a "ready" signal from the next stage
* Include valid/ready handshaking or use a FIFO
* Prevent data loss or duplication

This structure is known as a **pipelined system**: data flows through in steps, and each step operates independently. This makes the system faster and modular, but you must be careful with timing.


Architectural diagram:

![Pipeline Diagram](arch)

---

## ⚖️ Detailed Stage Descriptions (with Hints)

### Stage 1 — Bit Movement & Arithmetic

**What you know:**

* Uses the **first 3 bits of the key**
* These bits define how many positions the input is **shifted to the left**
* After the shift, something is **added** to the result

**Hint:**

> The number of positions you shift is also related to what you add to the input. Look at how the amount of movement connects to the arithmetic done after.

This stage is mainly combinational logic but may require a register if pipelined.

### Stage 2 — FSM-Controlled Logic

**What you know:**

* Uses the **next 2 bits of the key**
* The FSM has **4 different states**
* Each state applies a different operation to the data
* The current state and key bits determine the **next state**

**Hint:**

> The FSM chooses between operations like checking parity, bitwise logic (AND/OR), and extending the sign of certain bits. Focus on how the state machine is built, and how transitions happen.

You’ll need to design:

* A 2-bit current state register
* A next-state logic block
* Output logic depending on the current state

### Stage 3 — Cleanup Stage

**What you know:**

* Cleans up or reverses actions from the previous stage
* Based on the FSM output, it chooses which cleanup action to apply

**Hint:**

> If the FSM added a bit (like for parity), this is where it’s removed. If sign bits were extended, this is where they're dropped. Think of this as the final polish on the data before output.

---

## 🔢 Inputs and Outputs

You must design your top-level module with the following ports:

```verilog
module cryptoveril (
    input wire clk1,     // Stage 1 clock
    input wire clk2,     // Stage 2 clock
    input wire clk3,     // Stage 3 clock
    input wire rst,      // Global reset
    input wire [15:0] input_data, // Input data
    input wire [5:0] key_bits,    // 6-bit key
    output wire [15:0] output_data // Final result
);
```

You can also add internal signals for:

* Buffer registers
* Hshake signals (valid/ready)
* FSM state variables

---

## 🔮 What You’ll Learn

By the end of this challenge, you’ll have hands-on experience with:

* Bit-level manipulation in Verilog
* FSM design and control logic
* Building pipelined architectures
* Handling multiple clock domains
* Debugging waveform outputs in GTKWave

These are **essential skills** in real digital design and chip development workflows.

---

## 📊 How to Test Your Design

1. Run your Verilog design using **Verilator**
2. Create a `.vcd` file with signal traces
3. Open the trace in **GTKWave**
4. Step through each stage:

   * Check data entering/leaving each buffer
   * Verify that the FSM behaves correctly
   * Confirm that the output matches expectations

Example test cases and a testbench will be provided.

---

## 📥 What to Submit

At the end of the hackathon, submit:

* Your Verilog code (all modules and top-level)
* A testbench that exercises at least 3 different key-input pairs
* A `.gtkw` config file or screenshots showing signal flow
* (Optional) A document explaining your FSM design and pipeline choices

---

## ⭐ Bonus Points

* Add a **reverse module** that undoes all three stages and restores the original input
* Use parameterized buffer sizes for clock frequency flexibility
* Implement a visual trace of your FSM transitions

---

## 🏋️ Summary Checklist

* [ ] Implement 3-stage logic using the provided hints
* [ ] Use 3 different clocks
* [ ] Add buffers between each stage
* [ ] Simulate using Verilator
* [ ] Debug using GTKWave
* [ ] Submit working code and waveform screenshots

---

## 🌟 Final Words

This project is about **designing**, not just coding. It challenges you to think like a hardware engineer: moving bits, managing timing, and debugging waveforms. The logic is open-ended. You control the architecture, the flow, and the solution.

Everything is open-source. But the logic?

That’s yours to discover.

Good luck, and happy hacking! 🚀







