# Pseudorandom number generator:

**Pseudorandom number generators (PRNGs)** are algorithms or hardware circuits that produce sequences of numbers that appear random but are generated using a deterministic process, making them suitable for various applications in simulations, cryptography, and randomization.

- Use shift registers and XOR feedback.
- Generate seemingly random but deterministic sequences.
- Common in cryptography, data manipulation, and testing.
- Require an initial seed to start.
- Sequence length is determined by register size.
- Efficient and simple in hardware.
- Not suitable for strong cryptography alone.

```verilog
`timescale 1ns / 1ps

module lfsr_prng(
  input wire clk, rst,
  output wire [3:0] rand_out );

  reg [3:0] lfsr_state;  // 4-bit LFSR state register

  always @(posedge clk or posedge rst) begin
    if (rst) begin
      lfsr_state <= 4'b1001; // Initial state (can be any non-zero value)
    end else begin
      // XOR taps for a 4-bit LFSR: 4, 3
      lfsr_state <= {lfsr_state[2:0], lfsr_state[3] ^ lfsr_state[0]};
    end
  end

  assign rand_out = lfsr_state;

endmodule
```

# Pseudorandom number generator Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench_lfsr_prng();

  reg clk; 
  reg rst;
  wire [3:0] rand_out;

  lfsr_prng Dut( .clk(clk), .rst(rst), .rand_out(rand_out) );

  always begin
    #5 clk = ~clk; 
  end

  initial begin
    clk = 0;
    rst = 0;

    rst = 1;
    #10 rst = 0;

    $monitor("Time %t\t Output %b", $time, rand_out);

    #100 $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/r5sXdCc/random-num.png)

## Simulation
![Alt Text](https://i.ibb.co/QrMQ1NV/random-num-tb.png)
