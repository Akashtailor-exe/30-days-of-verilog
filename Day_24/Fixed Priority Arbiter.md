# Fixed Priority Arbiter:

- An **arbiter** is hardware logic that **selects one request from multiple competing requests** to be granted access to a shared resource. commonly used to manage access to shared interconnects, memory buses, caches, and other multi-master systems.
- A **fixed priority arbiter prioritizes requests** based on a pre-determined static **priority ordering**. The highest priority request is granted access first. If multiple requests of the same priority arrive, the numerically lower or earlier request is given priority.
- Fixed priority arbiters are useful when the relative importance of requests is known in advance. Critical traffic like interrupts can be assigned highest priority to minimize latency. **Lower priority requests may experience higher latencies or starvation.**

```verilog
`timescale 1ns / 1ps

module fixed_priority_arbiter(
    input clk, reset,
    input [7:0] req,
    output reg [7:0] grant); 

// grant[0] is the highest priority and grant[7] is the lowest priority.
    
    always @(posedge clk or posedge reset) begin
    
    if(reset) grant <= 8'b00000000;
    else begin
    
    if (req[0])
    grant <= 8'b00000001;
    
    else if (req[1])
    grant <= 8'b00000010;
    
    else if (req[2])
    grant <= 8'b00000100;
    
    else if (req[3])
    grant <= 8'b00001000;
    
    else if (req[4])
    grant <= 8'b00010000;
    
    else if (req[5])
    grant <= 8'b00100000;
    
    else if (req[6])
    grant <= 8'b01000000;
    
    else if (req[7])
    grant <= 8'b10000000;
    
    else
    grant <= 8'b00000000;
    
    end
    end
endmodule
```

# Fixed Priority Arbiter Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

reg clk;
reg reset;
reg [7:0] req;
wire [7:0] grant;

fixed_priority_arbiter dut (.clk(clk), .reset(reset), .req(req), .grant(grant));

initial begin
clk = 0;
forever #5 clk = ~clk;
end

initial begin 

reset = 1;
req = 0;

  #10 reset = 0;

  #10 req = 8'b11110000;
  
  #10 req = 8'b00001111;

  #10 req = 8'b10101010;

  #10 req = 0;
  
  #10 $finish;

end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/Q9xYqch/fixed-priority-arbiter.png)

## Simulation
![Alt Text](https://i.ibb.co/Q8bQMrf/fixed-priority-arbiter-tb.png)

### **Advantages:**

- Simple priority logic - easy to implement in hardware
- Predictable behavior - priority order is fixed and will not change dynamically
- Low latency for high priority requests - highest priority granted first
- Prevent starvation of highest priority requests

### **Disadvantages:**

- Lower priority requests may experience high latency or starvation
- Priority order cannot be changed dynamically based on current needs
- Priority conflicts may occur if multiple high priority requests arrive together
- Does not account for priority inversion - a high priority request may get blocked by a lower priority one

## More complex schemes like round-robin can be better when priorities are dynamic.
