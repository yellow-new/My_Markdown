# MATLAB C2000----- 例程（一）GPIO点灯

------

 官方链接[MATLAB 点灯](https://ww2.mathworks.cn/help/supportpkg/texasinstrumentsc2000/ug/getting-started-example.html)

## （一）模型

![](C:\My_Project_Markdown\Study_YellowNew\image\MATLAB_电灯1.png)

前面模块为时间模块 后面为gpio口 设置了翻转模式 这个为最简单的模式

![](C:\My_Project_Markdown\Study_YellowNew\image\MATLAB_电灯2.png)



## （二）烧入过程
1. "CTRL+E"打开设置

![](C:\My_Project_Markdown\Study_YellowNew\image\MATLAB电灯3.png)

2. 选择一下设置之后,准备烧入板卡

![](C:\My_Project_Markdown\Study_YellowNew\image\MATLAB电灯.png)

3. 等待一段时间会在你的工作路径下生成对应的文件当然会生成ccs代码

![](C:\My_Project_Markdown\Study_YellowNew\image\MATLAB电灯4.png)



##  （三）物理现象

![](C:\My_Project_Markdown\Study_YellowNew\image\动画.gif)



## (四）代码分析



```c
/*
 * File: ert_main.c
 *
 * Code generated for Simulink model 'c28379D_cpu1_blink'.
 *
 * Model version                  : 2.0
 * Simulink Coder version         : 9.5 (R2021a) 14-Nov-2020
 * C/C++ source code generated on : Wed Aug  4 22:46:51 2021
 *
 * Target selection: ert.tlc
 * Embedded hardware selection: Texas Instruments->C2000
 * Code generation objectives: Unspecified
 * Validation result: Not run
 */

#include "c28379D_cpu1_blink.h"
#include "rtwtypes.h"

volatile int IsrOverrun = 0;
static boolean_T OverrunFlag = 0;
void rt_OneStep(void)  		// 该函数放在定时器当中的
{
    //启延时作用
  /* Check for overrun. Protect OverrunFlag against preemption */
  if (OverrunFlag++) {
    IsrOverrun = 1;
    OverrunFlag--;
    return;
  }
//开启定时器中断
  enableTimer0Interrupt();
//io口电平翻转函数    
  c28379D_cpu1_blink_step();

  /* Get model outputs here */
  disableTimer0Interrupt();
  OverrunFlag--;
}

volatile boolean_T stopRequested; // 
volatile boolean_T runModel;
int main(void)
{
  float modelBaseRate = 1.0;
  float systemClock = 200;

  /* Initialize variables */
  stopRequested = false;
  runModel = false;
  c2000_flash_init();
  init_board();

#ifdef MW_EXEC_PROFILER_ON

  config_profilerTimer();

#endif

  ;
  rtmSetErrorStatus(c28379D_cpu1_blink_M, 0);
  c28379D_cpu1_blink_initialize();
  globalInterruptDisable();
  configureTimer0(modelBaseRate, systemClock);
  runModel =
    rtmGetErrorStatus(c28379D_cpu1_blink_M) == (NULL);
  enableTimer0Interrupt();
  globalInterruptEnable();
  while (runModel) {
    stopRequested = !(
                      rtmGetErrorStatus(c28379D_cpu1_blink_M) == (NULL));
  }

  /* Disable rt_OneStep() here */

  /* Terminate model */
  c28379D_cpu1_blink_terminate();
  globalInterruptDisable();
  return 0;
}

/*
 * File trailer for generated code.
 *
 * [EOF]
 */

```



​	详细代码需要自己区理解
