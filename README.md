# Encryption-Decryption-25


# 🔐 CRYPTOVERIL++: The Key-Controlled Bitstream Puzzle

Welcome to **CRYPTOVERIL++**, a hardware puzzle where you're given two things:  
- a stream of **16 bits of input data**,  
- and a **short key** that tells a hidden machine how to change that data.

Your challenge is to figure out how the system transforms the input using the key, and then implement this system using **Verilog**, **Verilator**, and **GTKWave**.

---

## 🎯 What You Need to Do

You’ll build a digital circuit with **three processing stages**.  
Each stage changes the input a little more, based on parts of the key.  
Your goal is to:
- Build the stages
- Get the output to match expected patterns
- Use GTKWave to check if it’s working

---

## 💡 Key Terms (But Easy)

- `input`: The original 16-bit data stream
- `key`: A short control value (less than 10 bits) that tells each stage what to do
- `stage`: A step in the process where the data is modified
- `pipelining`: A way to let each stage work at its own speed without waiting

---

## 🏗️ How Pipelining Works Here

Each of the three stages runs at a different clock speed:

- Stage 1 is the **fastest**
- Stage 2 is **slower** (1/3 the speed)
- Stage 3 is the **slowest** (1/3 of Stage 2)

To keep things moving smoothly:
- Each stage has a **buffer** — like a waiting room — so slower stages don’t block faster ones
- These buffers store data temporarily until the next stage is ready

This is called **pipelining**. It’s like a factory where each worker (stage) works at their own pace, and parts are passed through using boxes (buffers) between them.

---

## 🧠 How It Works (Without Giving Away the Secret)

There are three stages. Each one reads part of the key and processes the input based on that.

### 🔹 Stage 1 – Bit Movement
This stage moves bits around and does something else with the number of bits moved.  
💡 *Hint: Count how many bits are moved. That number matters.*

### 🔹 Stage 2 – Logic Machine (FSM)
This part has a small control system that switches between 4 different "modes" depending on the key. Each mode transforms the data in a different way.  
💡 *Hint: Some modes care about how many 1s and 0s are in the input.*

### 🔹 Stage 3 – Final Fix
This stage cleans up the data depending on what Stage 2 did. It removes or corrects something added earlier.  
💡 *Hint: The cleanup depends on the mode used in the previous stage.*

---

## 🧪 How to Test It

1. Run your code using **Verilator**  
2. View the results in **GTKWave**  
3. Make sure each stage transforms the data correctly  
4. Check that the final output matches what you expect

Bonus: Try building a **reverse version** that takes the final output and turns it back into the original input.

---

## 🧰 Tools Needed

- WSL/VirtualBox Ubuntu 22.04 LTS or lesser
- Verilator (for running your Verilog code)
- GTKWave (to see what’s happening inside your circuit)

---

## 📝 What to Submit

- Your Verilog code (all 3 stages)
- Screenshots from GTKWave showing your output works
- .vcd dump from your Verilator dump.
- Your reverse/decryption design

---


## 📌 Remember

Everything is open — no hidden modules, no secret files.  
But **how it works is for you to figure out**.  
Think logically, simulate carefully, and explore with GTKWave.

Good luck! 🔧💡






















