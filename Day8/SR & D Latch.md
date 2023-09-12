#  SR Latch
```verilog
`timescale 1ns / 1ps

module sr_latch (
    input S, 
    input R,
    output Q, 
    output Qbar
);

  reg Q, Qbar;

  always @(S, R) begin
    if (R == 1'b1) begin
      Q <= 1'b0;
      Qbar <= 1'b1;
    end else if (S == 1'b1) begin
      Q <= 1'b1;
      Qbar <= 1'b0;
    end
  end

endmodule
```

# SR Latch Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench_sr_latch;

  reg S;
  reg R;

  wire Q;
  wire Qbar;

  sr_latch dut ( .S(S), .R(R), .Q(Q), .Qbar(Qbar) );

  initial begin
    S = 0;
    R = 0;

    $monitor("  %0d  S %b R %b Q %b Qbar %b", $time, S, R, Q, Qbar);
    
    S = 0;R = 0; #10;
    S = 1;R = 0; #10;
    S = 0;R = 1; #10;
    S = 0;R = 0; #10;
    

    $finish; 
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/3zWwBkP/SR-Latch.png)

## Simulation
![Alt Text](https://i.ibb.co/FHqnxMg/SR-Latch-simu.png)


# D Latch:
```verilog
`timescale 1ns / 1ps

module D_Latch (
    input D,
    input EN,  
    output reg Q,   
    output Qbar 
);

assign Qbar = ~Q;

always @(D, EN)
begin
    if (EN)
        Q <= D;
end

endmodule
```

# D Latch Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

  reg D;
  reg EN;
  wire Q;
  wire Qbar;

  D_Latch dut ( .D(D), .EN(EN), .Q(Q), .Qbar(Qbar) );

  initial begin
    D = 0;
    EN = 0;

    $display("Time D EN Q Qbar");
    $monitor("%d %b %b %b %b", $time, D, EN, Q, Qbar);

    EN = 1;
    D = 1; #10;
    D = 0; #10;

    EN = 0;
    D = 1; #10;
    D = 0; #10;
    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/GWzg2J4/D-Latch.png)

## Simulation
![Alt Text](https://i.ibb.co/SQLxn7F/D-Latch-simu.png)
