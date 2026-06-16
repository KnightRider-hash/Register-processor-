> **⚠️ NOTE: This repository has been archived. This module has been successfully integrated into a complete 32-bit RISC-V CPU. Please see the master project here: [RV32I-Single-Cycle-Core](https://github.com/KnightRider-hash/RV32I-Single-Cycle-Core)**

# Register File — RISC-Style 32×32 Register Bank #
A synthesizable Verilog module implementing a 32-entry,
32-bit register file commonly used in RISC-style processor designs . 
Supports simultaneous dual-port read and single-port synchronous write, with hardwired zero at register x0.

## Port Description ##
| Port    | Direction | Width    | Description                                         |
|---------|-----------|----------|-----------------------------------------------------|
| `src1`  | Input     | `[4:0]`  | Address of the first source register (read port A)  |
| `src2`  | Input     | `[4:0]`  | Address of the second source register (read port B) |
| `dest`  | Input     | `[4:0]`  | Destination register address (write port)           |
| `data1` | Output    | `[31:0]` | Data read from register `src1`                      |
| `data2` | Output    | `[31:0]` | Data read from register `src2`                      |
| `rs`    | Input     | `[31:0]` | Data to be written to the destination register      |
| `rd`    | Input     | `1`      | Write enable signal (active high)                   |
| `clk`   | Input     | `1`      | Clock signal (write on rising edge)                 |

## Behavior ##
* Read (Asynchronous / Combinational)-
data1 and data2 are continuously driven based on src1 and src2.
Reading from register x0 (5'b00000) always returns 32'b0, regardless of any write — this mirrors the hardwired-zero convention in RISC-V.

* Write (Synchronous)-
On the rising edge of clk, if rd == 1 and dest != 0, the value on rs is written to x[dest].
Writes to x0 are ignored to preserve the hardwired-zero property.
