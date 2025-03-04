`timescale 1ns / 1ps

module MBA 
#(
    parameter LOGQ       = 64,
)(
    input                 clk,
    input                 rst,
    input  [LOGQ/4:0]     in_a,
    input  [LOGQ/4:0]     in_b,
   // input  [LOGQ-1:0]     q,
    output  reg [LOGQ/2:0] out_c
);

 reg [LOGQ/2:0] ans[LOGQ/8:0];
   reg [LOGQ/2:0] extended_a_r;
   reg [LOGQ/2-1: 0]extended_b_r; // Extended a_r
    integer i;
    //reg [2:0] lookup_tbl;
    reg [LOGQ/2:0] out1,out2,out3,out4;
    reg [ 2 : 0] lookme [LOGQ/8:0];
    
    /// First stage register inputs
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            extended_a_r <= 0;
            extended_a_r <= 0;
            end
        else 
            begin 
              extended_a_r <= {{LOGQ{1'b0}}, in_a}; 
             extended_b_r <= in_b;
                end
     end

    generate 
                for ( genvar m=0; m<= LOGQ/4 ; m= m+2) begin
                        always @ (posedge clk) begin
                        lookme[m/2] = {extended_b_r[m+1], extended_b_r[m], ((m == 0) ? 1'b0 : extended_b_r[m-1])};
                                 end
                        end
     endgenerate
   always@(posedge clk) begin
     for(i = 0; i < LOGQ/8; i = i+2) begin
                     case(lookme[i/2])
                         3'b001, 3'b010, 3'b101, 3'b110 : begin
                             ans[i/2] = extended_a_r << i;
                         end
                         3'b011,3'b100: begin
                             ans[i/2] = extended_a_r << (i + 1);
                         end
                         default: ;
                     endcase
                 end
             end
   always@ (posedge clk) begin
        for (i= LOGQ/8 ; i<= LOGQ/4 ; i = i+2) begin
            case(lookme[i/2])
                         3'b001, 3'b010, 3'b101, 3'b110 : begin
                             ans[i/2] = extended_a_r << i;
                         end
                         3'b011,3'b100: begin
                             ans[i/2] = extended_a_r << (i + 1);
                         end
                         default: ;
                     endcase
            end          
end
 always@(posedge clk) begin
  out1 =0;
     for(i = 0; i < LOGQ/8; i = i+2) begin
      case (lookme[i/2])
                      3'b001, 3'b010,3'b011: begin
                      out1 = out1 + ans[i/2];
                      end
                      3'b101, 3'b110, 3'b100  : begin
                      out1 = out1 - ans[i/2];
                      end
                     default: ;
                     endcase
               end
         end 

 always@(posedge clk) begin 
 out4  = 0 ;
    for (i =LOGQ/8; i <= LOGQ/4 ; i= i +2) begin
       case (lookme[i/2])
                      3'b001, 3'b010,3'b011: begin
                      out4 = out4 + ans[i/2];
                      end
                      3'b101, 3'b110, 3'b100  : begin
                      out4 = out4 - ans[i/2];
                      end
                     default: ;
                     endcase
               end
    
    end  
    
 always @ (posedge clk) begin
     out_c <= out1 +out4;
    //out_c <= out1 + out2 +out3 +out4 ;
 
 end
  endmodule




