# 🗳️ Digital Voting Machine with Secure Memory (Verilog / Digital Design)

## 📌 Overview

This project implements a **Digital Voting Machine (DVM)** using **Verilog HDL**.
It simulates an electronic voting system where votes are cast for three candidates, counted in real-time, and **persistently stored in secure on-chip memory registers** — mimicking the behaviour of EEPROM-backed storage.

The design demonstrates fundamental concepts of **sequential logic**, **state control**, **digital counters**, and **two-tier memory protection**.

---

## ⚙️ Features

* 🧑‍🤝‍🧑 **Multi-candidate voting** — supports Candidates A, B, and C simultaneously
* 🔢 **Real-time vote counting** — counts update on every rising clock edge
* 🔒 **Secure memory registers** — votes persist in internal `mem_A/B/C` registers even after a normal reset
* 🔄 **Two-tier reset system**
  * **Normal reset** — reloads the display counters from secure memory (counts are preserved)
  * **Secure (factory) reset** — wipes both the display counters and secure memory completely
* ⚡ **Mixed reset design** — `reset` is synchronous (clock-gated); `secure_reset` is asynchronous (triggers immediately outside the clock edge)
* 📊 **VCD waveform export** — compatible with GTKWave for waveform inspection

---

## 🛡️ Security & Memory

The module name `VotingMachineSecure` reflects a key design goal: **vote integrity even in the face of accidental resets**.

### How secure memory works

```
┌──────────────────────────────────────────────────────┐
│  Secure Memory (mem_A, mem_B, mem_C)  ← 4-bit each  │
│  Written on every vote; never cleared by normal reset│
└──────────────────┬───────────────────────────────────┘
                   │ reloaded on normal reset
┌──────────────────▼───────────────────────────────────┐
│  Display Counters (count_A, count_B, count_C)         │
│  Visible on outputs; reset by both reset types        │
└──────────────────────────────────────────────────────┘
```

| Signal         | Effect on display counters | Effect on secure memory |
| -------------- | -------------------------- | ----------------------- |
| `reset = 1`    | Reloads from secure memory | **No change**           |
| `secure_reset = 1` | Clears to zero         | **Clears to zero**      |

This two-level approach ensures that:
* A **power glitch or accidental button press** cannot erase recorded votes.
* An **authorised factory reset** (`secure_reset`) fully wipes the machine for a new election.

---

## 🧠 Working Principle

1. On each rising clock edge the module checks `secure_reset`, then `reset`, then the vote inputs — in that priority order.
2. A vote signal (`vote_A`, `vote_B`, or `vote_C`) increments both the corresponding secure memory register **and** the output counter atomically.
3. Only one vote is accepted per clock cycle (mutually exclusive `if/else if` chain).

---

## 🏗️ Design Details

### Inputs

| Signal         | Width  | Description                                      |
| -------------- | ------ | ------------------------------------------------ |
| `clk`          | 1 bit  | System clock (active on rising edge)             |
| `reset`        | 1 bit  | Normal reset — reloads counts from secure memory |
| `secure_reset` | 1 bit  | Factory reset — clears memory and counts         |
| `vote_A`       | 1 bit  | Cast a vote for Candidate A                      |
| `vote_B`       | 1 bit  | Cast a vote for Candidate B                      |
| `vote_C`       | 1 bit  | Cast a vote for Candidate C                      |

### Outputs

| Signal    | Width  | Description                    |
| --------- | ------ | ------------------------------ |
| `count_A` | 4 bits | Current vote count, Candidate A |
| `count_B` | 4 bits | Current vote count, Candidate B |
| `count_C` | 4 bits | Current vote count, Candidate C |

---

## 📐 Technical Specifications

| Parameter              | Value                               |
| ---------------------- | ----------------------------------- |
| HDL Language           | Verilog (IEEE 1364)                 |
| Counter width          | 4 bits per candidate (max 15 votes) |
| Secure memory width    | 4 bits per candidate                |
| Clock period (testbench) | 10 ns → 100 MHz equivalent        |
| Number of candidates   | 3 (A, B, C)                         |
| Reset types            | Normal + Secure (two-tier)          |
| Synthesis target       | Xilinx FPGA (Vivado flow)           |

---

## 📂 Project Structure

```
Digital-Voting-Machine-with-Secure-Memory/
│
├── Digital_Voting_Machine_MainModule.txt   ← Verilog RTL source
├── Digital_Voting_Machine_Testbench.txt    ← Verilog testbench
├── Digital Voting Machine.rar              ← Archived project files
├── Digital_Voting_Machine_Output.docx      ← Simulation output document
├── LICENSE                                 ← GNU GPL v3
└── README.md
```

---

## 🖥️ Simulation Results

Expected `$monitor` console output when running the testbench:

```
T=0    | A=0 | B=0 | C=0   ← initial state
T=10   | A=0 | B=0 | C=0   ← after factory reset
T=30   | A=1 | B=0 | C=0   ← vote_A cast
T=50   | A=1 | B=1 | C=0   ← vote_B cast
T=70   | A=1 | B=1 | C=1   ← vote_C cast
T=90   | A=2 | B=1 | C=1   ← second vote_A
T=130  | A=2 | B=1 | C=1   ← normal reset (counts reload from memory)
T=150  | A=2 | B=2 | C=1   ← vote_B cast again
T=190  | A=0 | B=0 | C=0   ← secure reset (all cleared)
```

### Waveform snapshot

| Time (ns) | count_A | count_B | count_C | Event               |
| --------- | ------- | ------- | ------- | ------------------- |
| 0         | 0       | 0       | 0       | Initialisation      |
| 30        | 1       | 0       | 0       | vote_A              |
| 50        | 1       | 1       | 0       | vote_B              |
| 70        | 1       | 1       | 1       | vote_C              |
| 90        | 2       | 1       | 1       | vote_A (2nd)        |
| 130       | 2       | 1       | 1       | Normal reset        |
| 150       | 2       | 2       | 1       | vote_B (2nd)        |
| 190       | 0       | 0       | 0       | Secure / factory reset |

---

## 🧪 Tools & Technologies

* **Verilog HDL** (IEEE 1364 / SystemVerilog compatible simulators)
* **ModelSim** or **Xilinx Vivado** — simulation and synthesis
* **Icarus Verilog (`iverilog`)** — free, open-source simulation
* **GTKWave** — open-source VCD waveform viewer

---

## ✅ Requirements

### Software (choose one simulator)

| Tool              | Version    | Notes                            |
| ----------------- | ---------- | -------------------------------- |
| Icarus Verilog    | ≥ 10.x     | Free, Linux/macOS/Windows        |
| ModelSim          | Any        | Included with Intel Quartus      |
| Xilinx Vivado     | 2020.x+    | For FPGA synthesis               |
| GTKWave           | ≥ 3.3      | Optional — waveform viewer       |

### Hardware (optional, for FPGA deployment)

* Xilinx Artix-7 / Spartan-6 or equivalent FPGA development board

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/Arighna2003/Digital-Voting-Machine-with-Secure-Memory.git
cd Digital-Voting-Machine-with-Secure-Memory
```

### 2️⃣ Rename Source Files

The source files use `.txt` extensions for portability. Rename them before simulation:

```bash
cp Digital_Voting_Machine_MainModule.txt voting_machine_secure.v
cp Digital_Voting_Machine_Testbench.txt  voting_machine_secure_tb.v
```

### 3️⃣ Simulate with Icarus Verilog (free & open-source)

```bash
# Compile design and testbench
iverilog -o voting_sim voting_machine_secure.v voting_machine_secure_tb.v

# Run simulation (produces VCD and console output)
vvp voting_sim
```

### 4️⃣ View Waveforms in GTKWave

```bash
gtkwave VotingMachineSecure.vcd
```

### 5️⃣ Simulate with ModelSim

```tcl
# In the ModelSim Tcl console:
vlog voting_machine_secure.v voting_machine_secure_tb.v
vsim VotingMachineSecure_TB
run -all
```

### 6️⃣ Simulate with Xilinx Vivado

1. Create a new **RTL project** in Vivado.
2. Add `voting_machine_secure.v` as a **design source**.
3. Add `voting_machine_secure_tb.v` as a **simulation source**.
4. Click **Run Simulation → Run Behavioural Simulation**.
5. Use the waveform window to inspect signals.

---

## 📚 Applications

* **Electronic voting kiosks** — tamper-resistant vote storage prototype
* **FPGA embedded systems** — demonstrates on-chip secure memory patterns
* **Digital design education** — sequential logic, counters, and reset hierarchies
* **Security-aware hardware design** — illustrates two-tier memory protection

---

## 🔮 Future Improvements

* 🪪 **Voter authentication** — unique voter ID prevents double voting
* 🖥️ **Display interface** — 7-segment or LCD to show live results
* 🔐 **Encryption** — hash-based vote integrity verification
* 📡 **Remote / IoT integration** — UART or Ethernet result transmission
* 🗂️ **Multi-election support** — configurable candidate count via parameters

---

## 🐛 Troubleshooting

| Problem | Likely Cause | Fix |
| ------- | ------------ | --- |
| `iverilog: command not found` | Icarus Verilog not installed | Install via `sudo apt install iverilog` (Linux) or visit the [Icarus Verilog GitHub releases](https://github.com/steveicarus/iverilog/releases) |
| Simulation exits immediately | Missing `$finish` timing or file paths wrong | Verify both `.v` files compiled together |
| VCD file not created | `$dumpfile` path issue | Run `vvp` from the same directory as source files |
| Counts don't increment | `vote_*` held high for only part of a clock cycle | Ensure vote signals span a full clock period (≥ 10 ns) |
| `secure_reset` doesn't clear memory | `secure_reset` is an **asynchronous** reset (in the sensitivity list as `posedge secure_reset`); it must actually pulse high to trigger | Verify the signal actually transitions from low to high; a signal held permanently high will not re-trigger |
| Counter overflows unexpectedly | More than 15 votes cast (4-bit limit) | Widen counter to `[7:0]` for larger elections |

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository on GitHub.
2. **Create a branch** for your feature or fix:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Commit** your changes with a clear message:
   ```bash
   git commit -m "Add: description of your change"
   ```
4. **Push** to your fork and open a **Pull Request** against `main`.
5. Ensure your Verilog compiles cleanly with `iverilog` before submitting.

Please keep code style consistent with the existing module (4-space indentation, descriptive signal names, inline comments).

---

## 👨‍💻 Author

**Agnik Maity**
B.Tech, Electronics & Communication Engineering
Institute of Engineering & Management (IEM), Kolkata

---

## 📜 License

This project is licensed under the **GNU General Public License v3.0**.
See the [LICENSE](LICENSE) file for full terms.

---

## ⭐ Support

If you found this project useful or educational, please consider giving it a ⭐ on GitHub — it helps others discover the project!
