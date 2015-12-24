#android 蓝牙控制继电器——单片机控制端
---
本文为博主[飞奔的蜗牛](https://huangxiankui.github.io/blog/index.html?diaries/index.md)原创文章，转载请加以说明

---
一直没怎么写博客，贴一个比较简单小例子(以前做的 android 通过wifi实现万能遥控器 中拆分出来的小案例）：
android客户端通过蓝牙协议，控制继电器（想控制其他玩意，自己设计）
![小作品展示](http://img.blog.csdn.net/20151224101946993)

---
今晚,先说硬件电路以及单片机部分(这个用51单片机的电路图真是简单到不能再简单了)。
![这个用51单片机的电路图真是简单到不能再简单了](http://img.blog.csdn.net/20151224102145958)
最小单片机系统板以及外围电路，这个电路非常简单（注意继电器的接法），不做过多介绍。

---
单片机端程序（蓝牙模块做串口使用）：
```c
/*********************************************************** 
C52与蓝牙连线： 
单片机程序需要复位一次 才能有效接收
注意对比特率的设置
蓝牙模块接法
 蓝牙  51
 RXD    TXD 
 TXD    RXD 
unsigned int tmp=0x00;  //初始tmp定义 主要表明tmp接收的是16进制数据
unsigned  char tmp 表明tmp接收的字符型数据
其他连线： 
P11接一个继电器 
蓝牙模块做串口使用

***********************************************************/  
#include <reg52.h>  
#include <intrins.h>  
unsigned int tmp=0x00;  //初始tmp定义 主要表明tmp接收的是16进制数据
//unsigned  char tmp 表明tmp接收的字符型数据
sbit ji=P1^1;     //继电器  
void init();  //初始化
void delay(unsigned int i);  //延时函数
void ctrl(); //根据接受信息做出操作  
void main()  
{  
    init(); 
    ji=0;
	
          while(1)  
          {  
		  //RI 用来接收和发送的标志位	RI==1，表示接收到了 RI==0，表示发送出去了
            
                  if(RI==1)                     // 是否有数据到来  
                  {  
                    
                  tmp = SBUF; 
				   RI = 0;                  // 暂存接收到的数据  
                    ctrl(); 			 //根据接受信息做出操作  
	  
      }            
          }  
		  }
  
 void init() //初始化  
 {  
  TMOD = 0x20;                        // 定时器1工作于8位自动重载模式, 用于产生波特率  
  TH1=0xFD;
  TL1=0xFD;                                // 波特率9600  
  SCON = 0x50;                        // 设定串行口工作方式1 允许接收,相当于下面三句  
  PCON =0x00;                        // 波特率不倍增  
  TR1 = 1;  
  EA=1;  
  ES=1;  
          
 }  
void delay(unsigned int i)  
{  
 unsigned char j;  
 for(i; i > 0; i--)  
 for(j = 200; j > 0; j--) ;  
}  
  
void ctrl()   //根据接受信息做出对应操作  
{  
  switch(tmp) //已经把SBUF中的数据赋值到tmp中了  
      {  
     case 0x01:  
       ji=1;              //收到字符1，继电器0关  
       break;       
     case 0x02:      //收到字符2，继电器开 
       ji=0;  
       break;  
    
     default:                    
        
		break;
    
      }  
} 
```