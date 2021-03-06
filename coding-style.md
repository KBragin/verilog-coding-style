# Verilog (SystemVerilog) Coding Style

## ������� � �������
������ - 2 �������. ���� (`\t`) ���������.

������ ����� ��� ����������� "��������".

������:
```systemverilog
if( a )
  b = c;
```

### �������
������ ������ ��������:
  - ����� ������������� `(` � ����� ������������� ������� `)` � ������������ `if`, `for`, `always`, `function`, `task`, ������� ���������� ��������
  - ����� � ����� ���������, ��������� ���� ��������� ( `&`, `|`, `&&`, `||`, `=`, `<=`, `+`)
  - ����� � ����� �������������� ���������� � ��������. ����������: ����� `,` � `;` ������ �� ��������.

�������:
```systemverilog
if( a > c )
```

```systemverilog
a = b && ( c > d );
```

```systemverilog
a = ( b == c ) ? ( y ) : ( z );
```

### ������ �������
����� ���������� ���� ���������� �������� ������ ��������.
��� `���������� �����` ���������� �����-�� ��������� ��� ������������ ��������, ������� ����� ���������� ���������� ��� ����.

������:

```systemverilog
...
  input        clk_i,
  input  [1:0] mode_i,
  input        mode_en_i,
  output [7:0] data_o,
  output       data_val_o
...
```

����� �� �����, ��� ���� ��� ������ ��������:
  - ������������� `clk_i`
  - ��������� ������-�� ������ ������ `mode_i` � ��� ���������� `mode_en_i`
  - ������ �������� ������ `data_o` � ���������� ����� ������� `data_valid_o`

��� ���������� ������ ���������� ������ ������ ������� ����� ����������:
```systemverilog
...
  input        clk_i,

  input  [1:0] mode_i,
  input        mode_en_i,

  output [7:0] data_o,
  output       data_val_o
...
```

## ������������
������������ ���������� ��� ���������� ������ ����.
������������ ������������ � ������� ��������. ���� (`\t`) ���������.

���������� ����������� ��������, ����������� � ����������� �� "����������" ��������.

### ������ #1
�����������:
```systemverilog
// BAD EXAMPLE
logic rd_stb; //buffer read strobe
logic [31:0] ram_data; //data from RAM block
logic [1:0]if_mode; //interface mode
```

���������:
```systemverilog
// GOOD EXAMPLE
logic        rd_stb;    // buffer read strobe
logic [31:0] ram_data;  // data from RAM block
logic [1:0]  if_mode;   // interface mode
```

### ������ #2
�����������:
```systemverilog
// BAD EXAMPLE
input clk_i,
input rst_i,
input [31:0] data_i,
input data_valid_i,
output logic [7:0] data_o,
output data_valid_o
```

���������:
``` systemverilog
// GOOD EXAMPLE
input                            clk_i,
input                            rst_i,

input         [31:0]             data_i,
input                            data_valid_i,

output  logic [7:0]              data_o,
output                           data_valid_o

```
����������:
  - �� ��������� ���������� ������� "�����������" ����� ��������, ������, ����������� ������ 
    ������ ��������, ���� ��� �������� ���������� (��� � ������� ����).

### ������ #3

�����������:
```systemverilog
// BAD EXAMPLE
assign next_len = len + 16'd8;
assign next_pkt_len = pkt_len + 16'd4;
```

���������:
```systemverilog
// GOOD EXAMPLE
assign next_len     = len     + 16'd8;
assign next_pkt_len = pkt_len + 16'd4;
```

### ������ #4
�����������:
```systemverilog
// BAD EXAMPLE
always_ff @( posedge clk_i )
  begin
    pkt_data_d1 <= pkt_data_i;
    pkt_empty_d1 <= pkt_empty_i;
  end
```

���������:
```systemverilog
// GOOD EXAMPLE
always_ff @( posedge clk_i )
  begin
    pkt_data_d1  <= pkt_data_i;
    pkt_empty_d1 <= pkt_empty_i;
  end
```

## ������� ������������� ����������� � ����������

### ���������� ���������
  - ���������� ������������ ���������� �/���/�� (`&&`/`||`/`!`) ��� �������� �����-�� ���������� ���������,
    � �� �� ��������� ������� (`&`/`|`/`~`).
  - ����� ���������, ���������, � ��. ������� � ������.

#### ������ #1
```systemverilog
logic data_ready;

assign data_ready = ( pkt_word_cnt > 8'd5 ) && ( !data_enable ) && ( pkt_len <= 16'd64 );
```

#### ������ #2

```systemverilog
always_comb
  begin
    if( data_enable && ( fifo_bytes_empty >= pkt_size ) )
      ...
  end
```

#### ������ #3

```systemverilog
assign start_stb_o = ( ( state == RED_S    ) && ( next_state != IDLE_S  ) ) ||
                     ( ( state == YELLOW_S ) && ( next_state != GREEN_S ) );
```

### `if-else`

```systemverilog
if( a > 5 )
  d = 5;
else
  d = 15;
```

### `if-else` ������ � `begin/end`

```systemverilog
if( a > 5 )
  begin
    c = 7;
    d = 5;
  end
else
  begin
    c = 3;
    d = 7;
  end
```

### ��������� `if-else`

```systemverilog
if( a > 5 )
  c = 7;
else
  if( a > 3 )
    c = 4;
  else 
    c = 5;
```

### ��������� �������� `?`

```systemverilog
assign y = ( a > c ) ? ( d ) : ( e );
```

������� ��������, ��� ������� � ���������� `d`, `e` ����� � ������� ������.

��� ���������� ������ (� ��������/���������) ����������� (� �� ������ ������� �������������) ������ � ��� ������:
```systemverilog
assign y = ( a > c ) ? ( cnt_gt_zero ):
                       ( cnt_le_zero );
```

### `case`

������ �� ��������� ����������� � ����� `begin/end` �����, �������
�������� ���� �� ����� ������ �������.

```systemverilog
case( opcode[1:0] )
  2'b00:
    begin
      next_state = OR_S;
    end

  2'b01:
    begin
      next_state = AND_S;
    end

  2'b10:
    begin
      next_state = NOT_S;
    end

  2'b11:
    begin
      next_state = XOR_S;
    end

  default:
    begin
      next_state = AND_S;
    end
endcase
``` 

���� `case` ������� � �� ������������� ������� ��������� ����������� � `begin/end` �����, 
�� ����������� ������ ��� (��� ���������� ����� � ������):

```systemverilog
case( opcode[1:0] )
  2'b00:   next_state = OR_S;
  2'b01:   next_state = AND_S;
  2'b10:   next_state = NOT_S;
  2'b11:   next_state = XOR_S;
  default: next_state = AND_S;
endcase
```

��������, ��� ����� ��������� �� `next_state` ��� ���������� ������.

### `function` � `task`

```systemverilog
function int calc_sum( input int a, int b );
  return a + b;
endfunction
```

```systemverilog
task receive_pkt( output packet_t pkt );
  ...
endtask
```

��� ������� ���������� ���������� ��������� ������� � �������� � �������:
```systemverilog
task some_magic( 
  input   int        a,
  input   bit [31:0] data,
  output  packet_t   pkt 
);
...
endtask
```
������, ���� � ��� ������� ���������� ����������, �������� ���-�� �� ������� �� ���...

### ��� ���� ������ 
```systemverilog
if( condition1 )
  begin
    for( int i = 0; i < 10; i = i + 1 )
      statement1;
  end
else
  begin
    if( condition2 )
      statement2;
    else
      if( condition3 )
        statement3;
      else
        statement4;
  end
```

## �����������
����������� ������� �� ���������� �����.
����� ����� ����������� `//` �������� ���� ������.

���������� ������ ����������� ����� ��� ������, ������� �� ������ ��������:
```systemverilog

// current packet word number
always_ff @( posedge clk_i or posedge rst_i )
  if( rst_i )
    pkt_word <= 16'd0;
  else
    // reset counter at last valid packet word
    if( pkt_valid && pkt_eop )
      pkt_word <= 'd0;
    else
      if( pkt_valid )
        pkt_word <= pkt_word + 1'd1;
```

����������:
  - ��� �������� ������������ ���������� ������������ ������� ������� � 
    ������������ ������������ "������ ����������� ���, �������� ���". 
    ������, ������� ���������� ����, ���������� *���* ���������� ����������� 
    �����������, �� �� ���� �������������� ������ ���� � ������ ������� 
    (� ���� ����� ������ ������ �� ������ ���������).

## ������������ ���������� � �������

- ����� �������� ����������, �������, ������ � ������� - `snake_case`, �.�. ����� ��� ��������
  ����������� `_`, � �ӣ ������� ���������� �������.

  ������:
    - `pkt_anlz`
    - `is_ptp_pkt`
    - `super_task`
  
- �������� ������� (�� ��� �� ����� `_`) ������� ������ ���������, ���������, �������, ��������� FSM

  ������:
    - `parameter A_WIDTH = 9;`
    - `define CRC_LEN 32'd4`

- ��� ���������� ������ �������� �� ����������. 
  ������� �������� ��������� ������� �, ��������, ��������� �������� �������� (`rd_addr` ����� ��� `ra`).

- �������� ������ ������ ��������� ������� `_i` ��� �������, `_o` ��� ��������, `_io` ��� 
  ��������������� ��������.

## �������� ������
- ��� �������� ������ ������������ ������� `clk`. 
- ��� ���������� ������ ������ ����� ����������� � ����� ������. 
- ���� ���������� �������, ����� ������� �������������� ���� ����, �� ���������� ������������ ������ `clk_XmABC`, ���:
  - `X` - �������� ������� � ���
  - `ABC` - �������� ����� (������� ��� �����), ���� ��� �� ����� ����.
    �������:
    - `2.048 ��� -> clk_2m048`
    - `156.25 ��� -> clk_156m25`
    - `250 ��� -> clk_250m`
- ����������� ������� ��� �������, ����� ������ ���� (���������� ������ ��������) � ���� ��������, 
  ������� ���� �������������� ������, ������� ����� �������� �� ������� 100 ���, �� ��� �������� ����� ������ 
  ������� ������������ ����� �������� `clk_i`, � ���-�� "������" ���������� ������ 100 ���:

  ```systemverilog
    .clk_i    ( clk_100m      ),
    ....
  ```

- ���� ����� �����-�� �������������� � ����������� �� �����, �� ���������� ������� �������� � ������:

  ```systemverilog
    parameter CLK_FREQ_HZ = 100_000_000;
  ```

  � ����� ��� ������������ � ������.

- ��� �������� ������ ������������� ������ �� �������������� ������ `posedge`. 
  ����������: ���������� ���������� ����������� (��������, DDR).

������������� **�����������**:
  - ������������ ������ ����� ��� �������� ������� ��� �������� �������������� ������, ����:

    ```systemverilog
       assign my_super_clk = clk_i && ( cnt == 8'd4 );
    ```

## �������� �������
���� ��� ���� ������ (������):
  - ����������� `rst`
  - ����������  `srst`

���� ����� ��� ������������ ��� ���� ��� ������������ ������:
```systemverilog
...

  input     clk_i,
  input     rst_i,

...
```

�������� ������� ��� ������ ������� `1`, �.�. ����� `rst_i == 1'b1`, �� ����� ��������� � ������. 

�������� �������� � ����������� �������:
```
always_ff @( posedge clk_i or posedge rst_i )
  if( rst_i )
    cnt <= 8'd0;
  else
    ...
```

�������� �������� � ���������� �������:
```
always_ff @( posedge clk_i )
  if( srst_i )
    cnt <= 8'd0;
  else
    ...
```

�������� �������� � ���������� � ����������� �������:
```
always_ff @( posedge clk_i or posedge rst_i )
  if( rst_i )
    cnt <= 8'd0;
  else
    if( srst_i )
      cnt <= 8'd0;
    else
    ...
```

��� ����������� ������� ������������ **����������� �����** ��� ���������� ������ � ��������� ���������.

������������� **�����������**:
  - ������ ���������� � ����������� ����� ��� �� ��������� ��������, ��� � �� �� ������������
  - ��������� ������ ��� �������. ����������: ������ ��� ���������/������ (������ �������������� ������).
  - ����������� �����-�� ����������� � ����������� �������, ����:
    ```systemverilog
      logic [7:0] cnt;
      assign my_rst = rst_i || ( cnt == 8'd5 );

      always_ff @( posedge clk_i or posedge my_rst )
        if( my_rst )
          cnt <= 8'd0;
        else
          ...
    ```

## �������� ��������� �������� (FSM)

��� �������� ��������� �������� ������������ ��������� ����� (� ��������� ��� ����� ��� ��������� `FSM 3 process`):

```systemverilog
enum logic [1:0] { IDLE_S,
                   RUN_S,
                   WAIT_S } state, next_state;

always_ff @( posedge clk_i or posedge rst_i )
  if( rst_i )
    state <= IDLE_S;
  else
    state <= next_state;

always_comb
  begin
    next_state = state;

    case( state )
      IDLE_S:
        begin
          if( ... )
            next_state = RUN_S;
        end
      
      RUN_S:
        begin
          if( ... ) 
            next_state = WAIT_S;
          else
            if( )
              next_state = IDLE_S; 
        end

      WAIT_S:
        begin
          if( ... )
            next_state = RUN_S;
        end

      default:
        begin
          next_state = IDLE_S;
        end

    endcase
  end

// some logic, that generates output values on state and next_state signals

```

���������:
  - `state` ���������� ������� ���������, `next_state` - ��, ������� ����� � `state` �� ��������� �����.  
  - `IDLE_S` - ��������� ���������, ������� ��� ������������� �� ����� ������������ ������ � `default` � `case`-�����.
     ��� �������� ������� �������, ��� ��� ��������� ������ ���� ������ � `enum` ������.

- ����� ��������� FSM ����������� �������� ������� � ������ ��������� ������� `_S` (`IDLE_S`, `TX_S`, `WAIT_S` � �.�.).

������������� **�����������**:
  - ������ � ����� ������ ������ ������ ��������� ��������
  - � �����, ��� ���������� ���������� �� `next_state` ������ ���������� ��� �����-���� ��������
  - ����������� �����-���� ������ ��������, ����� �������� ��������� (`==`, `!=`) � ����������� `state`, `next_state`, ��������:

    ```systemverilog
      assign b = ( state > RED_S );
      assign c = ( state + 3'd2 ) > ( IDLE_S );
    ```

## ���������� ����������

��������� ����������� � ������. 
������� �������: ���� ������, package, ���������� - ���� ����. �������� ����� - �������� ����� ������. 

- ����������:
  - Verilog: `.v`
  - SystemVerilog: `.sv`
  - Verilog Header: `.vh`
  - SystemVerilog Header: `.svh`
  - VHDL: `.vhd`

- ����� ���������� ������ ���������� package ������� �������� ���������� `_pkg`: `func_pkg.sv`
- ����� ���������� ������ �����������, �������, �����, ������� ������������ � �������� ��� ������ include, ������� ��������� ����������� .vh ������
`defines.vh`.

## �������� ���������� ������/���������

�����:
  - � ����� ����� ���������� ��������� ������������ ������ � **����** ����������.
  - ������������ � ���������� ����� ����������� ������ � ����� �����.
  - ���������� ����� ��������� ������ �������� (������ � �����).
  
### �������������� ������
  - ��� �������� �������������� ���� ����� ������������:
    - ���� `always_comb`
    - ����������� (continuous) ������������ `assign`
  - ����������� ������������ ������ ����������� ������������ `=`.

������:
```systemverilog
always_comb
  begin
    a = b + d;
  end
```

������������� **�����������**:
  - ��������� ������ ��� "��������" ����������:
  ```systemverilog
  logic a = b && d;
  ```

  - ��������� ��������� �������� ��� �������������� ������:
  ```systemverilog
  logic [7:0] a = 8'd5;

  assign a = b + d;
  ```

### ��������
  - ������������ ���� `always_ff`
  - ����������� ������������ ������ ������������� ������������ (`<=`).

������: 
```systemverilog
logic [31:0] data_d1;
logic [7:0]  cnt;

always_ff @( posedge clk_i )
  begin
    data_d1 <= data_i;
  end

always_ff @( posedge clk_i or posedge rst_i )
  if( rst_i )
    cnt <= '0;
  else
    if( cnt == 8'h92 )
      cnt <= 'd0;
    else
      cnt <= cnt + 1'd1;
```

#### �������� ��������� � ��������� ��������� ���������

```systemverilog
logic [7:0] cnt = 8'd5;

always_ff @( posedge clk_i or posedge rst_i )
  if( rst_i )
    cnt <= 8'd5;
  else
    ...
```

### ������� (�����)

������������ ���� `always_latch`.

������:
```systemverilog
always_latch
  begin
    if( en )
      data = data_i;
  end
```

������������� **�����������**
  - ������������/��������� ����� � ���������� ������. 

## �������� � ���������� �������

### �������� ������
������ ������ ����������� � ��������� �����.

���� `some_module.sv`:

```systemverilog
module some_module #(
  parameter DATA_WIDTH = 32,
  parameter CNT_WIDTH  = 8
)(
  input                         clk_i,
  input                         rst_i,

  input        [DATA_WIDTH-1:0] data_i,
  input                         data_val_i,

  output logic [CNT_WIDTH-1:0]  cnt_o
);

// some code...

endmodule
```

�.�.:
  - �������� ���������� � �������� ���������� � ������� (2 �������).

  - ��� �������� ���������� � �������� �ӣ ������������� �� ��������:
    - �������� �������� 
    - ���������� ������ 
    - ����������������� ����� ���� `parameter`, `input`, `output`
    - ���� `=` � ����������

  - ������� �� ����������� � ����. �� �������, ������� ��������� ���������� ���������
    ������ ��������� ������ �������.

  - ��������������� ������� ��������� ������� �������, ����� ��������, ������,
    ������������ ������ ����������� ������� �����������, ������� ������ ����.

  - ������ � �������� �������� ������ ���� �������� ������/�������������� � �������.  

### ������� ������

```systemverilog
some_module #(
  .DATA_WIDTH                             ( 64                ),
  .CNT_WIDTH                              ( 4                 )
) some_module (
  .clk_i                                  ( rx_clk            ),
  .rst_i                                  ( main_rst          ),

  .data_i                                 ( rx_data           ),
  .data_val_i                             ( rx_data_val       ),

  .cnt_o                                  ( cnt               )
);  
```

�.�.:
  - ��� ����������� ������� ��� ��������������� ��������� �������� ������ (2 �������).
  - C�����, �����, ������� �������������.
  - ��� �������� ���������� ������������ `#`, � �� `defparam`.
  - ������ �������, ������� �������� ���������� ���������� (��� �������, ������� ������ ���� ������) 
    ����������� ��� �� ��� � � �������� ������.
  - ���� ����� ��������� ������ ���������� ��� �� ��� � ��� ������; 
    ���� ����������� ������ ������ ���������, �� �� �������� 
    ����������� ���������� `_inst0`, `_inst1` ( ���� ������ `0`, `1` ) � �.�. 
    ������ ������ �������� ���������� ������� ������� � ������ ����������� ������� �������������
    ���� ����� ���� ����� �������, ��� �������� � ���������� ��������� � ModelSim ��� TimeQuest.
    ������� ����������� ��������(!) ������� ����: `main_engine` ��������� �� `me`. ������ ���
    ������������� ���� �� ��������������.

## �������� ����������

���������� � ������ "���������" ����� �������� �������� ������ � �� ������ "������" � ����� ���������.

```systemverilog
  output logic abc_o
);

logic [7:0]  cnt;
logic        cnt_en;

logic [63:0] pkt_data;
// ... some more signals

// work with signal starts here
```

- ��������� ����������, ��� � ������� ������ ���������� ��������� �������� ������ �������. 
- ����������� ��������� ������� "�����" � ��� ������, ��� ��� �������������, ��������:
```systemverilog
logic [2:0] vlan_mpls_cnt;
// more signals

// some code
always_comb
  begin
    if( vlan_mpls_cnt > 2 )
      ...
  end

// calc vlan_mpls_cnt
logic [1:0] vlan_cnt;
logic [1:0] mpls_cnt;

assign vlan_cnt = vlan[0] + vlan[1] + vlan[2];
assign mpls_cnt = mpls[0] + mpls[1] + mpls[2];

assign vlan_mpls_cnt = vlan_cnt + mpls_cnt;
```

## ������

- ������������� ����������� ������������� � ����������� ������� � �������
  �������� �������. ����������� ����� ���� ����������� ����������, ��� �����
  ������� ���������� ������������� (sram interface, � �������). �����������
  ������� �������� �������� �� ������ FPGA, �� �� ������� ������ �������� (� top-�����)
  ��� ������ ���� ����������������.

- ��� ������� ��������������� ���, ����������� �������� ������ ������������� ��
  ������� ������ �������� (top-�����).

- � RTL ��������� ������������ ��� logic (�� ����������� ��� �������, �����
  ���������� ������������� ��������). ��� �������� ������� ������, ��������� �
  ������������� ����������� �� ����� ���������� � ������, ��������� �����������
  ������������� (��� ��� logic ���������������� �������������� ���������).

- ������������ ������������� ����������� � �������� �������� ������� �������.
  ��� ������� ������������ �������� ������ ����������.

- ��� ��������� � �������������� ���� �� ����������� ������� ��������� �
  ��������� ������. ��� �������� �������������� ��� ���������� �� ������ �����.

- ��� ���������������� (`define) ������ ������������ � ��������� ����� ���� �
  ������ �������� ������ ��������.

- Hard-code ���������� �, �� �����������, ������ ������ ���� ���������������.
  ���������� �������� � ��� �������, ����� ����������������� ������ ���������
  ����� ����������� � ��������. ������� ���������� - ������ �� ������ ���������
  ����������� ����� ���������� ������ �������� ���������, �.�. ������ ���������
  ��������� ����������. 
