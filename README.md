# 4-bit CPU in Verilog

A simple 4-bit CPU designed and implemented in Verilog as a learning project to understand fundamental computer architecture concepts.

## About This Project

I built this as my first project with verilog. It was quite intresting to make but i didn't really make this i just had the idea and AI did the rest. 

**Purpose:** To understand the fundamental components of a CPU and how they work together to execute instructions.

## Features

- **4-bit data path** - operates on 4-bit values
- **4 general-purpose registers** (R0, R1, R2, R3)
- **8-bit instruction format** - simple and easy to understand
- **7 operations:** ADD, SUB, AND, OR, XOR, NOT, MOV
- **External program loading** - programs loaded from `.mem` files
- **256 instruction capacity** - 8-bit program counter

##  Architecture

### Instruction Format (8 bits)
```
[7:5] - Opcode (3 bits)
[4:3] - Destination Register (2 bits)
[2:1] - Source Register (2 bits)
[0]   - Unused
```

### Instruction Set

| Opcode | Mnemonic | Operation                         | Description |
|--------|----------|-----------------------------------|-------------|
| 000    | ADD      | R[dest] = R[dest] + R[src]        | Addition    |
| 001    | SUB      | R[dest] = R[dest] - R[src]        | Subtraction |
| 010    | AND      | R[dest] = R[dest] & R[src]        | Bitwise AND |
| 011    | OR       | R[dest] = R[dest] \| R[src]       | Bitwise OR  |
| 100    | XOR      | R[dest] = R[dest] ^ R[src]        | Bitwise XOR |
| 101    | NOT      | R[dest] = ~R[dest]                | Bitwise NOT |
| 110    | MOV      | R[dest] = R[src]                  | Move/Copy   |
| 111    | ---      | undefined                         | Reserved    |

### Components

- **ALU (Arithmetic Logic Unit)** - Performs arithmetic and logic operations
- **Register Bank** - 4 registers for data storage with dual-read, single-write ports
- **Program Counter** - Tracks the current instruction address
- **Instruction Memory** - Stores the program (256 x 8-bit)
- **Control Unit** - Decodes instructions and generates control signals

### Block Diagram
```
     ┌─────────────┐
     │ Program     │
     │ Counter     │
     └──────┬──────┘
            │
     ┌──────▼──────┐
     │ Instruction │
     │ Memory      │
     └──────┬──────┘
            │
     ┌──────▼──────┐
     │ Control     │
     │ Unit        │
     └──┬────┬──┬──┘
        │    │  │
   ┌────▼────▼──▼────┐
   │  Register Bank  │
   └────┬────────┬───┘
        │        │
     ┌──▼────────▼──┐
     │     ALU      │
     └──────┬───────┘
            │
      (Result back to registers)
```

##  Project Structure
```
4bit-cpu/
├── src/
│   ├── ALU4.v
│   ├── registers.v
│   ├── program_counter.v
│   ├── instruction_memory.v
│   ├── control_unit.v
│   └── cpu_top.v
├── sim/
│   └── cpu_tb.v
├── programs/
│   └── program.mem
├── README.md
└── .gitignore
```

##  Documentation

### Prerequisites
- Xilinx Vivado (tested on version 2025.x) or any Verilog simulator
- Basic understanding of digital logic

### Running the Simulation

1. **Create a new Vivado project** or import files into your simulator

2. **Add all `.v` files** to your project:
   - Design sources: `ALU4.v`, `registers.v`, `program_counter.v`, `instruction_memory.v`, `control_unit.v`, `cpu_top.v`
   - Simulation sources: `cpu_tb.v`

3. **Create a program file** (`program.mem`) in your project directory:
```
   11000010
   00001000
   01010010
   10111000
```

4. **Run behavioral simulation**

5. **Check the console output** to see register values and instruction execution

### Writing Programs

Programs are written in binary, one instruction per line in `program.mem`.

**Example program:**
```
11000010    // MOV R0, R1  (Copy R1 to R0)
00001000    // ADD R1, R0  (R1 = R1 + R0)
01010010    // AND R2, R1  (R2 = R2 & R1)
10111000    // NOT R3, R0  (R3 = ~R3)
```

**Instruction encoding helper:**
```
Opcode (3 bits) | Dest Reg (2 bits) | Source Reg (2 bits) | Unused (1 bit)

Example: ADD R2, R1
  000 (ADD) | 10 (R2) | 01 (R1) | 0
  = 00010010
```

##  Example Test

The testbench initializes registers and runs a simple program:

**Initial values:**
- R0 = 5
- R1 = 3
- R2 = 12
- R3 = 2

**After execution:**
- Results depend on your program!

##  What I Learned

- How a CPU executes instructions (fetch-decode-execute cycle)
- Verilog HDL syntax and simulation
- Register file design with multiple read ports
- ALU implementation
- Control unit logic and instruction decoding
- How all CPU components interconnect

##  Future Improvements

Potential enhancements for this project:

- [ ] Add HALT instruction to stop execution
- [ ] Implement Load Immediate (LDI) instruction
- [ ] Add flags (Zero, Carry, Negative)
- [ ] Implement conditional branching (JZ, JNZ, JMP)
- [ ] Add data memory (RAM) with LOAD/STORE instructions
- [ ] Expand to 8-bit data path
- [ ] Synthesize and run on FPGA hardware
- [ ] Add I/O peripherals (LEDs, buttons)

##  Notes

- This is a **learning project** - designed for education, not production use
- The design follows RISC principles - simple, uniform instructions
- All operations complete in one clock cycle (single-cycle design)
- Registers are general-purpose (no special register 0 behavior)

##  Acknowledgments

- Built with guidance from AI
- Inspired by classic educational CPU designs like MIPS and educational processors
