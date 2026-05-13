# 4-bit and 8-bit Arithmetic Logic Unit

A combinational ALU implemented in Verilog, supporting 8 operations
selected by a 3-bit opcode, with zero and carry status flags.

## Interface

| Signal | Width | Direction | Description |
|--------|-------|-----------|-------------|
| `a` | 4 (or 8) | input | First operand |
| `b` | 4 (or 8) | input | Second operand |
| `opcode` | 3 | input | Operation selector |
| `result` | 4 (or 8) | output | Operation result |
| `zero` | 1 | output | High when result == 0 |
| `carry` | 1 | output | Arithmetic carry / shift-out bit |

## Operations

| Opcode | Operation | Description |
|--------|-----------|-------------|
| 3'b000 | ADD | result = a + b, carry on overflow |
| 3'b001 | SUB | result = a - b, carry on borrow |
| 3'b010 | AND | result = a & b |
| 3'b011 | OR | result = a \| b |
| 3'b100 | XOR | result = a ^ b |
| 3'b101 | NOT | result = ~a (b ignored) |
| 3'b110 | SHL | result = a << 1, carry = a[MSB] |
| 3'b111 | SHR | result = a >> 1, carry = a[0] |

## Files

- `alu.v` — ALU module
- `alu_tb.v` — Self-checking testbench
- `waveforms/` — Simulation waveform screenshots

## Running the simulation

On EDA Playground:
1. Open [project link]
2. Click "Run"
3. Testbench reports PASS/FAIL for 24 test cases

Locally with Icarus Verilog:
```bash
iverilog -o alu_sim alu.v alu_tb.v
vvp alu_sim
```

## Verification

The testbench covers all 8 opcodes with at least 3 input combinations each,
including edge cases (a=0, b=0, maximum values, intentional overflow conditions).
All 24+ test cases pass with PASS/FAIL output via `$display`.

## Notes on design choices

- Used a `case` statement with a `default` clause to prevent latch inference
- Carry computed via 5-bit (or 9-bit) intermediate to capture overflow naturally
- All outputs declared `reg` since they are assigned inside an `always @(*)` block
