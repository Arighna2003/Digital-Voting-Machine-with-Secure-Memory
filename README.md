# 🗳️ Digital Voting Machine (Verilog / Digital Design)

## 📌 Overview

This project implements a **Digital Voting Machine (DVM)** using **Verilog HDL**.
It simulates a simple electronic voting system where votes are cast for three candidates and counted in real-time.

The design demonstrates fundamental concepts of **sequential logic**, **state control**, and **digital counters**.

---

## ⚙️ Features

* 🧑‍🤝‍🧑 Voting for multiple candidates (A, B, C)
* 🔢 Real-time vote counting
* 🔄 Reset functionality
* ⚡ Efficient sequential logic design
* 📊 Simulation waveform and synthesis outputs included

---

## 🧠 Working Principle

* Each candidate is assigned a vote input signal:

  * **A → Candidate A**
  * **B → Candidate B**
  * **C → Candidate C**
* When a vote signal is triggered:

  * The respective candidate's counter increments
* The system maintains vote counts until reset

---

## 🏗️ Design Details

### Inputs

* `clk` → Clock signal
* `reset` → Resets all vote counts
* `vote_A`, `vote_B`, `vote_C` → Voting inputs

### Outputs

* `count_A`, `count_B`, `count_C` → Vote counts

---

## 📂 Project Structure

```id="w8yx2d"
Digital-Voting-Machine/
│
├── src/
│   └── voting_machine.v
│
├── simulation/
│   └── waveform.png
│
├── synthesis/
│   ├── rtl_schematic.png
│   └── synthesized_design.png
│
├── output/
│   └── results.txt
│
└── README.md
```

---

## 🖥️ Simulation Results

| Time (ns) | Count A | Count B | Count C |
| --------- | ------- | ------- | ------- |
| 0         | 0       | 0       | 0       |
| 25000     | 1       | 0       | 0       |
| 45000     | 1       | 1       | 0       |
| 65000     | 1       | 1       | 1       |
| 85000     | 2       | 1       | 1       |
| 135000    | 2       | 2       | 1       |
| 160000    | 0       | 0       | 0       |

---

## 🧪 Tools & Technologies

* Verilog HDL
* ModelSim / Vivado (Simulation)
* Xilinx Vivado (Synthesis)

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

```bash id="9c1p2x"
git clone https://github.com/your-username/digital-voting-machine.git
cd digital-voting-machine
```

### 2️⃣ Run Simulation

* Open in ModelSim / Vivado
* Compile the design
* Run simulation:

```bash id="3x8kdl"
run -all
```

---

## 📚 Applications

* Electronic voting systems (basic model)
* Digital counter design learning
* FPGA-based embedded systems
* Educational demonstration of sequential circuits

---

## 🔮 Future Improvements

* Voter authentication system
* Display interface (7-segment / LCD)
* Secure voting mechanism
* Remote/IoT-based voting integration

---

## 👨‍💻 Author

**Agnik Maity**
Institute of Engineering & Management, Kolkata

---

## 📜 License

This project is licensed under the **GNU License**.

---

## ⭐ Support

If you found this useful, consider giving it a ⭐ on GitHub!
