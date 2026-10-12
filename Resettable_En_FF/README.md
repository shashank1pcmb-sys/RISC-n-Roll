# Resettable Flip-Flop with Enable

Design a 8-bit parameterized flip-flop with synchronous enable and asynchronous reset.

### Module Specification

```SystemVerilog
module flopenr #(parameter WIDTH = 8)(
    input  logic              clk,   // Clock
    input  logic              reset, // Asynchronous reset
    input  logic              en,    // Enable signal
    input  logic [WIDTH-1:0]  d,     // Data input
    output logic [WIDTH-1:0]  q      // Data output
);

    always_ff @(posedge clk, posedge reset) begin
        if (reset)    q <= '0;   // reset has highest priority
        else if (en)  q <= d;    // update only when enabled
        // else: q holds its value
    end

endmodule
```

### Working

- Asynchronous reset: sets `q` to `0` when `reset` is high. 
- Enable (`en`): updates output `q` only when `en` is high.

> [!TIP] 
> Consider the order of reset and enable in the `always_ff` block to ensure correct behavior.

### Fixed Interface

> [!IMPORTANT]
> The module name, port list, and port widths shown above are a fixed contract for this course. Do not rename the module, rename/reorder/add/remove ports, or change port widths.
