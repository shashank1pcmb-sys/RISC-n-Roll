# 3:1 Multiplexer

Design an 8-bit parameterized 3:1 Multiplexer.

### Module Specification

```SystemVerilog
module mux3 #(parameter WIDTH = 8)(
    input  logic [WIDTH-1:0] d0,  // Input 0
    input  logic [WIDTH-1:0] d1,  // Input 1
    input  logic [WIDTH-1:0] d2,  // Input 2
    input  logic [1:0]       s,   // 2-bit Select line
    output logic [WIDTH-1:0] y    // Output
    );
    // s = 00 -> d0 ; s = 01 -> d1 ; s = 10 -> d2 ; s = 11 -> d2 (unused code)
    always_comb begin
        case (s)
            2'b00:   y = d0;
            2'b01:   y = d1;
            2'b10:   y = d2;
            default: y = d2;   // covers 2'b11 (and X/Z) so no latch is inferred
        endcase
    end

endmodule

### Fixed Interface

> [!IMPORTANT]
> The module name, port list, and port widths shown above are a fixed contract for this course. Do not rename the module, rename/reorder/add/remove ports, or change port widths.
