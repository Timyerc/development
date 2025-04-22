# EHDS-01-REMOTER-3 2.4G 通信协议

@Justin 2024/6/21

## 数据包

\<length>, \<preamble>, \<reserved>, \<command>, \<rolling_code_h>, \<rolling_code_l>, \<checksum>, \<epilog>

| 项 | 描述 | 备注 |
| ----------- | ----------- | ----------- |
| length | 数据包长度 | 0x07 |
| preamble | 包头 | 0xAA |
| reserved | 保留字节 | 发送0x00 |
| command | 遥控键码 | 参考下方说明 |
| rolling_code_h | 滚码高字节 | |
| rolling_code_l | 滚码低字节 | |
| checksum | 数据包校验和 | XOR of \<reserved>,\<command>, <br>\<rolling_code_h>, \<rolling_code_l> |
| epilog | 包尾 | 0xFF |

### \<command> 遥控键码说明

按键位置对应的功能和键码如下：

|  |  |  |
| :-----------: | :-----------: | :-----------: |
| | 自动向上<br>0x12||
|  自动向左<br>0x1A|  | 自动向右<br>0x1E |
|  | 向上<br>0x05 |  |
| 向左<br>0x07 | 启停<br>0x08 | 向右<br>0x09 |
|  | 向下<br>0x18 |  |
| 喷水模式切换<br>0x01 |  | 手动喷水<br>0x02 |

## 跳频说明

遥控器使用三个频点，分别是：

2432MHz, 2452MHz, 2471MHz

遥控器发送周期为4ms，且发送完一包后切换为下一个频点，使用如上三个频点循环发送。

## 芯片初始化参考

使用低码率62.5Kbps

```

HS_CSN=0;          
rf_buf=0x3D;
HS6220_Writebyte();
rf_buf=0x20;
HS6220_Writebyte();
HS_CSN=1;         

HS_CSN=0;         
rf_buf=0x3C;
HS6220_Writebyte();
rf_buf=0x00;         
HS6220_Writebyte();
HS_CSN=1;          

HS6220_CE_Low();
Delay_ms();

HS_CSN=0;          
rf_buf=0x20;
HS6220_Writebyte();
rf_buf=0x8A;    
HS6220_Writebyte();  /
HS_CSN=1;          
Delay_ms();
Delay_ms();

HS_CSN=0;          
rf_buf=0x23;
HS6220_Writebyte();
rf_buf=0xAC;
HS6220_Writebyte();
HS_CSN=1;         
Delay_ms();  

HS_CSN=0;          
rf_buf=0x3D;
HS6220_Writebyte();
rf_buf=0x10;
HS6220_Writebyte();
HS_CSN=1;          
Delay_ms();

bank1_switch();

HS_CSN=0;          
rf_buf=0x3F;
HS6220_Writebyte();
rf_buf=0x20;
HS6220_Writebyte();
HS_CSN=1;          

HS_CSN=0;          
rf_buf=0x33;        
HS6220_Writebyte();
rf_buf=0x01;        
HS6220_Writebyte();
HS_CSN=1;          

bank0_switch();

HS6220_CE_High();
Delay_ms();
HS6220_CE_Low();

data_buf=0;
B_LEAV_FG=0;
Init_cnt_leav=0;
while(!data_buf.5)  
{
    Delay_ms();
    HS_CSN=0;        
    rf_buf=0x06;
    HS6220_Writebyte();
    HS6220_Readbyte();
    data_buf=rf_buf;
    HS_CSN=1; 
    Init_cnt_leav++;
    .wdreset;
    if(Init_cnt_leav>12)  
    {
        Init_cnt_leav=0;   
        B_LEAV_FG=1;
        .wdreset;
    }
}


HS_CSN=0;              
rf_buf=0x26;
HS6220_Writebyte();
rf_buf=0x47;
HS6220_Writebyte();
HS_CSN=1;              

bank1_switch();

HS_CSN=0;             
rf_buf=0x23;
HS6220_Writebyte();
rf_buf=0x20;
HS6220_Writebyte();
rf_buf=0x98;
HS6220_Writebyte();
rf_buf=0x75;
HS6220_Writebyte();
HS_CSN=1;             

bank0_switch();

HS_CSN=0;           
rf_buf=0x2A;
HS6220_Writebyte();
rf_buf=0x55;
HS6220_Writebyte();
rf_buf=0x42;
HS6220_Writebyte();
rf_buf=0x9C;
HS6220_Writebyte();
rf_buf=0x8F;
HS6220_Writebyte();
rf_buf=0xC9;
HS6220_Writebyte();
HS_CSN=1;            

HS_CSN=0;           
rf_buf=0x30;
HS6220_Writebyte();
rf_buf=0x55;
HS6220_Writebyte();
rf_buf=0x42;
HS6220_Writebyte();
rf_buf=0x9C;
HS6220_Writebyte();
rf_buf=0x8F;
HS6220_Writebyte();
rf_buf=0xC9;
HS6220_Writebyte();
HS_CSN=1;             

Clear_tx_fifo();
Clear_rx_fifo();
Clear_rf_status();

HS_CSN=0;            
rf_buf=0x3D;
HS6220_Writebyte();
rf_buf=0x10;
HS6220_Writebyte();
HS_CSN=1;              

HS_CSN=0;           
rf_buf=0x21;
HS6220_Writebyte();
rf_buf=0x00;
HS6220_Writebyte();
HS_CSN=1;             

HS_CSN=0;              
rf_buf=0x20;
HS6220_Writebyte();
rf_buf=0x8A;       //
HS6220_Writebyte();
HS_CSN=1;              

HS_CSN=0;               
rf_buf=0x31;
HS6220_Writebyte();
rf_buf=0x08;
HS6220_Writebyte();
HS_CSN=1;               

HS_CSN=0;         
rf_buf=0x25;
HS6220_Writebyte();
rf_buf=0x20;
HS6220_Writebyte();
HS_CSN=1;      

HS_CSN=0;       
rf_buf=0x22;
HS6220_Writebyte();
rf_buf=0x0B;
HS6220_Writebyte();
HS_CSN=1;       
```

## 对应SOC发射芯片

GH8266