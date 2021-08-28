---
title: MATLAB C2000 例程（三） ---CLA与CPU交互控制LED闪烁
date: 2021-8-24
tags:
- c2000
- simulink	
---





#  一 . CLA  Introduction

---



   	CLA为C2000为了减少CPU负荷而研发的并行**协处理器**

![cla1](C:\My_Project_Markdown\Study_YellowNew\image\cla1.png)

## 1.CLA Tasks---CLA任务

-  ### CLA有八个任务（Task1[优先级最高]-Tack8），存在与CPU交互的RAM空间。

- ### 其程序与数据存放在LS0-LS5 RAM中，并且有两个128字节的==CPU to CLA（CLA to CPU）==的消息寄存器。

##  2.Interrupt trigger---触发条件

- ### Peripheral interrupt trigger（外部中断触发）

  > 每个Task都可以通过中断源触发
  >
  > 每个Task都是向**DmaClaSrcSelRegs.CLA1TASKSRCSELx[TASKx]**写入相应的Value

![cla1](C:\My_Project_Markdown\Study_YellowNew\image\cla2.png)

**Table 5-1** 为不同触发源对应的Value（部分）

- ### Software Trigger（软件触发）

  > 使用IACK指令或者写MIFRC寄存器
  >
  > IACK #0x0001  //0001 给寄存器写0 触发Task1
  >
  > IACK #0x0003  //0011 给寄存器写0 1触发Task1 Task2

- ### Background Task   (后台任务)

  > Task 8 作为后台任务 一直在运行当中



## 3.Typical CLA Initialization Sequence (CLA初始化过程)

![cla3](C:\My_Project_Markdown\Study_YellowNew\image\cla3.png)

##  4.CLA Illegal Opcode Behavior  (CLA非法指令)

- ###  The CLA will halt with the illegal opcode in the D2 phase of the pipeline as if it were a breakpoint. This will
  occur whether CLA breakpoints are enabled or not.

- ### The CLA will issue the task-specific interrupt to the PIE

- ### The CLA will issue the task-specific interrupt to the PIE.

  # 二 . SImulink CLA for blighting an LED

  ---

  

  
  
  ![cla4](C:\My_Project_Markdown\Study_YellowNew\image\cla4.png)
  
  
  
  ## 1.[烧入过程：详细见上一篇文章](https://yellownew.cn/2021/08/21/GPIO%20%E7%82%B9%E7%81%AF/#more)
  
   
  
  ## 2.Simulink  and Code  分析
  
  ### CLA Task 
  
  simulink ：
  
  ![](C:\My_Project_Markdown\Study_YellowNew\image\cla5.png)
  
  
  
  code：
  
  ```c
  相对应来就看很容易看出器对应逻辑
  /* CLA Interrupt block */
  __interrupt void Cla1Task1 ( void )
  {
    /* Call the system: <Root>/cla_subsystem */
    /* __mdebugstop(); */  
  
    /* Output and update for function-call system: '<Root>/cla_subsystem' */
    {
      real32_T rtb_Sum;
  
      /* Sum: '<S2>/Sum' incorporates:
       *  Delay: '<S2>/Delay'
       */
      rtb_Sum = input1 - fb_delay; //延时函数
  
      /* RelationalOperator: '<S2>/Relational Operator' incorporates:
       *  Constant: '<S2>/Constant'
       *  DataStoreWrite: '<S2>/Data Store Write'
       */
      Cla_out = (rtb_Sum > 0.5F); //高低电平
  
      /* Update for Delay: '<S2>/Delay' */
      fb_delay = rtb_Sum;
    }
  }
  
  ```
  
  ### Cla_out
  
  simulink:
  
  ![](C:\My_Project_Markdown\Study_YellowNew\image\cla6.png)
  
  
  
  code :
  
  ```c
  void isr_int11pie1_task_fcn(void)
  {
    if (1 == runModel) {
      /* Call the system: <Root>/System executes at completion of CLA Task1 */
      {
        /* S-Function (c28xisr_c2000): '<Root>/C28x Hardware Interrupt' */
  
        /* Output and update for function-call system: '<Root>/System executes at completion of CLA Task1' */
  
        /* DataStoreRead: '<S1>/Data Store Read' */
        c28379D_cpu1_blink_cla_B.input1_l = Cla_out; 
  
        /* S-Function (c280xgpio_do): '<S1>/Digital Output' */
        {
          if (c28379D_cpu1_blink_cla_B.input1_l)
            GpioDataRegs.GPASET.bit.GPIO31 = 1;
          else
            GpioDataRegs.GPACLEAR.bit.GPIO31 = 1;
        }
  
        /* End of Outputs for S-Function (c28xisr_c2000): '<Root>/C28x Hardware Interrupt' */
      }
    }
  }
  ```
  
  程序思路大致是:
  
  在Timer0中断函数中，手动触发CLA（利用汇编语句）。在CLA_Task1中会执行相应的模型算法，指向完毕之后。后进入CLA中断函数，在CLA中断函数中根据CLA运算结果，控制GPIO的翻转。
  
  
  
  # 	三.  Summary
  
  ---
  
  ### CLA对于dsp来说是非常重要的东西，他可以大大的提高dsp的运算能力。还需要多多理解
  
  ### Matlab生成的程序，格式比较固定，多看几个就能大致明白结构。以后可能就是更多的是关于Simulink中模块的学习了！

