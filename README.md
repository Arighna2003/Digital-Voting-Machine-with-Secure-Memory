# Digital Voting Machine (Verilog / Digital Design)

## Overview

This repository contains a **Digital Voting Machine (DVM)** implemented in **Verilog HDL**. It models a basic electronic voting system where votes are cast for three candidates and counted in real time.

The project is intended for learning and demonstration of:
- sequential logic design
- state/control logic
- synchronous counters

## Features

- Voting for three candidates (A, B, C)
- Real-time vote counting
- Reset to clear all counts
- Simulation waveform and synthesis outputs included

## Working Principle

- Each candidate has a dedicated vote input signal: `vote_A`, `vote_B`, `vote_C`.
- When a vote signal is asserted, the corresponding counter increments on the active clock edge.
- Counts are retained until `reset` is asserted.

## Interface

### Inputs

| Signal | Description |
|---|---|
| `clk` | Clock signal |
| `reset` | Resets all vote counts |
| `vote_A` | Vote input for candidate A |
| `vote_B` | Vote input for candidate B |
| `vote_C` | Vote input for candidate C |

### Outputs

| Signal | Description |
|---|---|
| `count_A` | Vote count for candidate A |
| `count_B` | Vote count for candidate B |
| `count_C` | Vote count for candidate C |

## Project Structure

```
Digital-Voting-Machine-with-Secure-Memory/
├── src/
│   └── voting_machine.v
├── simulation/
│   └── waveform.png
├── synthesis/
│   ├── rtl_schematic.png
│   └── synthesized_design.png
├── output/
│   └── results.txt
└── README.md
```

## Simulation Results

Example observed counts during simulation:

| Time (ns) | Count A | Count B | Count C |
|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 |
| 25000 | 1 | 0 | 0 |
| 45000 | 1 | 1 | 0 |
| 65000 | 1 | 1 | 1 |
| 85000 | 2 | 1 | 1 |
| 135000 | 2 | 2 | 1 |
| 160000 | 0 | 0 | 0 |

## Tools & Technologies

- Verilog HDL
- ModelSim and/or Vivado (simulation)
- Xilinx Vivado (synthesis)

## Getting Started

### Clone the repository

```bash
git clone https://github.com/Arighna2003/Digital-Voting-Machine-with-Secure-Memory.git
cd Digital-Voting-Machine-with-Secure-Memory
```

### Run simulation (generic)

1. Open the project in ModelSim or Vivado.
2. Compile the design and testbench.
3. Run the simulation (ModelSim example):

```tcl
run -all
```

> If you have a specific testbench/top module name, add it here for a one-command run.

## Applications

- Educational digital-design example for synchronous counters
- Basic model for an electronic voting system
- FPGA learning project

## Future Improvements

- Voter authentication
- Display interface (7-segment / LCD)
- Secure voting mechanism (e.g., vote locking/debouncing, access control)
- Remote/IoT-based voting integration

## Author

**Arighna Bhattacharjee**  
Institute of Engineering & Management, Kolkata

## License

License information is currently listed as "GNU License". Consider adding a `LICENSE` file and specifying the exact license (e.g., GPL-3.0).

## Support

If you found this useful, consider giving it a ⭐ on GitHub!
