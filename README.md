

3 workshop projects, each one has a sim and deployment component:

1. Blinky
Introduce

1. Blinky with PWM breathing

1. VGA with static output
  a. stripes
  b. box

1. VGA with dynamic output from buttonpress
  a. box
  b. sprite?




outline

1. Intro

    a. Make sure tooling is installed for all participants. Would be good if this repo had a script that could check build, sim, and deploy.
    a. Poll group on experience
    a.


2. Very brief intro to systemverilog
Start by describing basic digital logic:
  - wires / busses
  - flip-flops
  - logic gates
  - arithmetic operators
  - multiplexers

Develop intuition by drawing a few circuits and asking students what happens when certain signals change

  a. A single flip-flop with differing values at the input

  a. Two cascaded flip-flops with changing values at the input

  a. An edge detector that pulses high on a posedge

  a. A single flip-flop with an incrementing feedback path

  a. A single flip-flop with an incrementing feedback path and a reset

  a. A programmable PWM generator

Might be good to do live demonstrations of this with Logisim

Use a restricted subset of the language. Only use `always @(posedge x)` blocks with `<=` assignments.

Main language constructs to introduce:
  - `logic [y:x]`
  - `always_ff @(posedge x)`
  - ` <= `, arithmetic operations (`&`, `|`, `!`, `~`, `+`, `-`, `*`)
  - `if ()`
  - `begin` / `end`

Quiz students - show code snippets and then ask them what it will synthesize into.

  a.
  ``` Verilog
  logic [7:0] a;
  ```

  b.
  ``` Verilog
  logic [7:0] a;
  always_ff @(posedge clk) begin
      a <= a + 1;
  end
  ```

  c.
  ``` Verilog
  logic [7:0] a;
  always_ff @(posedge clk) begin
      if (reset) begin
          a <= 0;
      end else begin
          a <= a + 1;
      end
  end
  ```

  d.
  ``` Verilog
  logic [7:0] a;
  always_ff @(posedge clk) begin
      a <= a + 1;
      if (reset) begin
          a <= 0;
      end
  end
  ```

  e.
  ``` Verilog
  logic a;
  logic b;
  logic c;
  always_ff @(posedge clk) begin
      a <= signal;
      b <= a;

      c <= a & !b;
  end
  ```

  f.
  ``` Verilog
  logic winner;
  logic [1:0] state;
  always_ff @(posedge clk) begin
      if (state == 0) begin
          if (signal_a) begin
              state <= 1;
          end
      end else if (state == 1) begin
          if (signal_a) begin
              state <= 0;
          end else if (signal_b) begin
              state <= 2;
          end
      end else if (state == 2) begin
          if (signal_a) begin
              state <= 0;
          end else if (signal_b) begin
              state <= 3;
          end
      end else if (state == 3) begin
          if (signal_b) begin
              state <= 0;
              winner <= 1;
          end else if (signal_a) begin
              state <= 0;
          end
      end
  end
  ```

  f.
  ``` Verilog
  logic a, a_q, b, b_q;
  logic a_rose, b_rose;
  always_ff @(posedge clk) begin
      a <= a_in;
      a_q <= a;
      a_rose <= a & !a_q;

      b <= b_in;
      b_q <= b;
      b_rose <= b & !b_q;
  end

  logic winner;
  logic [1:0] state;
  always_ff @(posedge clk) begin
      if (state == 0) begin
          if (a_rose) begin
              state <= 1;
          end
      end else if (state == 1) begin
          if (a_rose) begin
              state <= 0;
          end else if (b_rose) begin
              state <= 2;
          end
      end else if (state == 2) begin
          if (a_rose) begin
              state <= 0;
          end else if (b_rose) begin
              state <= 3;
          end
      end else if (state == 3) begin
          if (b_rose) begin
              state <= 0;
              winner <= 1;
          end else if (a_rose) begin
              state <= 0;
          end
      end
  end