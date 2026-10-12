# Data Memory (DMEM)

Implement the Data Memory for the single-cycle RISC-V processor.

### Module Specification

```SystemVerilog
module dmem(
    input  logic        clk,
    input  logic        we,
    input  logic [31:0] a,   // Address
    input  logic [31:0] wd,  // Write data
    output logic [31:0] rd   // Read data
);

    logic [31:0] RAM [63:0];   // 64 words, 32 bits each

    // Combinational read, word-aligned (a is a byte address)
    assign rd = RAM[a[7:2]];

    // Synchronous write on rising clock edge
    always_ff @(posedge clk) begin
        if (we) RAM[a[7:2]] <= wd;
    end

endmodule
```

### Guidelines

- Addressing is **byte-addressable CPU addressing**: `a` is a byte address.
- Memory stores **32-bit words** and contains **64 words**.
- For aligned accesses, the word index is `a / 4`.
- Writes occur on the **rising edge of `clk`** when `we` (write enable) is high.
- Reads are **combinational**.

### Fixed Interface

> [!IMPORTANT]
> The module name, port list, and port widths shown above are a fixed contract for this course. Do not rename the module, rename/reorder/add/remove ports, or change port widths.
