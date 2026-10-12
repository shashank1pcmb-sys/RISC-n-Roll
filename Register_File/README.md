# Register File

Design a 32×32 three-ported register file with two read ports and one write port.

The register file contains **32 registers, each 32 bits wide**.

### Module Specification

```SystemVerilog
module regfile(
    input  logic        clk,   // clock
    input  logic        we3,   // write enable for port 3
    input  logic [4:0]  a1, a2, a3, // read addresses (a1,a2), write address (a3)
    input  logic [31:0] wd3,       // write data for port 3
    output logic [31:0] rd1, rd2   // read data outputs
);

    logic [31:0] rf [31:0];   // 32 registers, 32 bits each

    // Synchronous write (register 0 is never written)
    always_ff @(posedge clk) begin
        if (we3 && (a3 != 5'd0))
            rf[a3] <= wd3;
    end

    // Combinational reads (register 0 always reads as 0)
    assign rd1 = (a1 != 5'd0) ? rf[a1] : 32'd0;
    assign rd2 = (a2 != 5'd0) ? rf[a2] : 32'd0;

endmodule
```

### Working

- **Read Ports:**
    
    - `rd1` reads `rf[a1]`
        
    - `rd2` reads `rf[a2]`
        
    - Reads are **combinational**, i.e., reflect memory instantly.
        
- **Write Port:**
    
    - `rf[a3] <= wd3` on rising edge of `clk` **if `we3` is high**.
        
- **Register 0:**
    
    - Always hardwired to 0, cannot be written.

### Fixed Interface

> [!IMPORTANT]
> The module name, port list, and port widths shown above are a fixed contract for this course. Do not rename the module, rename/reorder/add/remove ports, or change port widths.
