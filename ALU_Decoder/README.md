# ALU Decoder

Design the ALU Control Unit that generates the ALU operation signals based on instruction fields and `ALUOp`.

### Module Specification

```SystemVerilog
module aludec(
    input  logic        opb5,      // instruction bit [5] of opcode
    input  logic [2:0]  funct3,    // funct3 field of instruction
    input  logic        funct7b5,  // bit 5 of funct7 field
    input  logic [1:0]  ALUOp,     // high-level ALU operation
    output logic [2:0]  ALUControl // ALU operation code
);

    logic rtypesub;
    assign rtypesub = funct7b5 & opb5;   // 1 only for R-type SUB

    always_comb begin
        case (ALUOp)
            2'b00:   ALUControl = 3'b000;   // LW, SW -> add
            2'b01:   ALUControl = 3'b001;   // BEQ    -> subtract
            2'b10: begin                    // R-type / I-type arithmetic
                case (funct3)
                    3'b000:  ALUControl = rtypesub ? 3'b001 : 3'b000; // SUB : ADD/ADDI
                    3'b010:  ALUControl = 3'b101;                     // SLT, SLTI
                    3'b110:  ALUControl = 3'b011;                     // OR,  ORI
                    3'b111:  ALUControl = 3'b010;                     // AND, ANDI
                    default: ALUControl = 3'b000;                     // unsupported funct3
                endcase
            end
            default: ALUControl = 3'b000;   // ALUOp = 11 (unused)
        endcase
    end

endmodule
);
```

### Working

- Determines the **ALU operation** (`ALUControl`) based on the instruction fields and `ALUOp`.
- Supports **R-type, I-type arithmetic, and branch instructions**.
- Must correctly differentiate between **add/subtract** for R-type instructions.

### ALUControl Mapping (Locked Baseline ISA)

The table below defines the required `ALUControl` value for every baseline instruction that reaches the ALU:

| Instruction | ALUControl |
|---|---|
| ADD, ADDI, LW, SW | 000 |
| SUB, BEQ | 001 |
| AND, ANDI | 010 |
| OR, ORI | 011 |
| SLT, SLTI | 101 |

### Fixed Interface

> [!IMPORTANT]
> The module name, port list, and port widths shown above are a fixed contract for this course. Do not rename the module, rename/reorder/add/remove ports, or change port widths.
