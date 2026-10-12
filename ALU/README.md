# ALU

Design a 32-bit Arithmetic Logic Unit (ALU) supporting basic operations.

### Module Specifications

```SystemVerilog
module alu(
    input  logic [31:0] a, b,          // operands
    input  logic [2:0]  alucontrol,    // operation selector
    output logic [31:0] result         // operation result
);

    always_comb begin
        case (alucontrol)
            3'b000:  result = a + b;                                   // Add
            3'b001:  result = a - b;                                   // Subtract
            3'b010:  result = a & b;                                   // AND
            3'b011:  result = a | b;                                   // OR
            3'b101:  result = ($signed(a) < $signed(b)) ? 32'd1 : 32'd0; // SLT (signed)
            default: result = 32'd0;                                   // unused codes
        endcase
    end

endmodule
);
```

### ALU Control Table

|alucontrol|Operation|
|---|---|
|000|Add|
|001|Subtract|
|010|AND|
|011|OR|
|101|Set Less Than (SLT)|

### Fixed Interface

> [!IMPORTANT]
> The module name, port list, port widths, and ALU control table above are a fixed contract for this course. Do not rename the module, rename/reorder/add/remove ports, change port widths, or redefine the `alucontrol` encodings.
