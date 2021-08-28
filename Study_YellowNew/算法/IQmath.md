---
title: C2000---IQmath
date: 2021-8-23
tags:
- c2000	
---

# 一、Introduction--IQmath

<font color=#999AAA >Ti提供将浮点数转化为定点数的库，可以提高运算速度

TI手册中的原话：

```css
Texas Instruments TMS320C28x IQmath Library is collection of highly optimized and high precision mathematical functions for C/C++ programmers to seamlessly port a floating-point algorithm into fixed point code on TMS320C28x devices. These routines are typically used in computationally intensive real-time applications where optimal execution speed and high accuracy is critical. By using these routines you can achieve execution speeds considerable faster than equivalent code written in standard ANSI C language. In addition, by providing ready-to-use high precision functions, TI IQmath library can shorten significantly your DSP application development time.
```



# 二、Using the IQmath Library
## 1.IQmath Arguments and Data Types

<font color=#999AAA >IQ1---IQ30 定义了不同大小和精度的数据类型



```c
typedef long _iq; /* Fixed point data type: GLOBAL_Q format */  // 这个为全局变量 需要单独定义
typedef long _iq30; /* Fixed point data type: Q30 format */
typedef long _iq29; /* Fixed point data type: Q29 format */
typedef long _iq28; /* Fixed point data type: Q28 format */
typedef long _iq27; /* Fixed point data type: Q27 format */
typedef long _iq26; /* Fixed point data type: Q26 format */
typedef long _iq25; /* Fixed point data type: Q25 format */
typedef long _iq24; /* Fixed point data type: Q24 format */
typedef long _iq23; /* Fixed point data type: Q23 format */
typedef long _iq22; /* Fixed point data type: Q22 format */
typedef long _iq21; /* Fixed point data type: Q21 format */
typedef long _iq20; /* Fixed point data type: Q20 format */
typedef long _iq19; /* Fixed point data type: Q19 format */
typedef long _iq18; /* Fixed point data type: Q18 format */
typedef long _iq17; /* Fixed point data type: Q17 format */
typedef long _iq16; /* Fixed point data type: Q16 format */
typedef long _iq15; /* Fixed point data type: Q15 format */
typedef long _iq14; /* Fixed point data type: Q14 format */
typedef long _iq13; /* Fixed point data type: Q13 format */
typedef long _iq12; /* Fixed point data type: Q12 format */
typedef long _iq11; /* Fixed point data type: Q11 format */
typedef long _iq10; /* Fixed point data type: Q10 format */
typedef long _iq9; /* Fixed point data type: Q9 format */
typedef long _iq8; /* Fixed point data type: Q8 format */
typedef long _iq7; /* Fixed point data type: Q7 format */
typedef long _iq6; /* Fixed point data type: Q6 format */
typedef long _iq5; /* Fixed point data type: Q5 format */
typedef long _iq4; /* Fixed point data type: Q4 format */
typedef long _iq3; /* Fixed point data type: Q3 format */
typedef long _iq2; /* Fixed point data type: Q2 format */
typedef long _iq1; /* Fixed point data type: Q1 format */
```

## 2.IQmath Data type: Range & Resolution

<font color=#999AAA > 给出了每种数据类型的范围以及精度

特别提醒了 IQNsin, IQNcos, IQNatan2, IQNatan2PU, IQatan的函数是不支持**Q30**格式的

![image-20210814102534781](C:\Users\yello\AppData\Roaming\Typora\typora-user-images\image-20210814102534781.png)

## 3.Calling an IQmath Function from C

- <font color=#999AAA > 在文件中包含IQmath.h文件

- 将代码与IQmath.h连接

- 在CMD文件中放置IQmath代码段

  

  ```c
  #include<IQmathLib.h>
  #define PI 3.14159
  _iq input, sin_out;
  void main(void )
  {
  /* 0.25 x PI radians represented in Q29 format */
  input=_IQ29(0.25*PI);
  sin_out =_IQ29sin(input);
  }
  ```

##  4.IQmath Naming Conventions

	对于名字的命名 规则的介绍

```c
 GLOBAL_Q function, that takes input/output in GLOBAL_Q format
C-Code Examples:
 _IQsin(A) /* High Precision SIN */
 _IQcos(A) /* High Precision COS */
 _IQrmpy(A,B) /* IQ multiply with rounding */
 _IQmpy(A,B) /* IQ multiply */
    
 Q-format specific functions to cater to Q1 to Q30 data format.
    
C-Code Examples:
 _IQ29sin(A) /* High Precision SIN: input/output are in Q29 */
 _IQ28sin(A) /* High Precision SIN: input/output are in Q28 */
 _IQ27sin(A) /* High Precision SIN: input/output are in Q27 */
 _IQ26sin(A) /* High Precision SIN: input/output are in Q26 */
 _IQ25sin(A) /* High Precision SIN: input/output are in Q25 */
 _IQ24sin(A) /* High Precision SIN: input/output are in Q24 */
```



![image-20210814104701624](C:\Users\yello\AppData\Roaming\Typora\typora-user-images\image-20210814104701624.png)

## 5.Selecting the GLOBAL_Q format

#### 		CASE 1 :

默认Global_q格式设置为Q24。编辑“iqmathlib.h”标题文件以根据需要修改此值，用户可以从Q1到Q29选择为Global_Q格式。修改此值

意味着所有Global_Q函数将使用此Q格式进行输入/输出，除非在源代码中覆盖此符号定义。

```C
#ifndef GLOBAL_Q
#define GLOBAL_Q 24 /* Q1 to Q29 */
#endif
```

#### CASE 2 :

完整的系统由各种模块组成。某些模块可能需要不同的精度，然后是系统的其余部分。在这种情况下，我们需要过度乘以在“iqmathlib.h”文件中

定义的global_q，并使用本地q格式。这可以通过在Include语句之前定义模块的源文件中的Global_q常数来轻松完成

```C
#define GLOBAL_Q 27 /* Set the Local Q value */
#include <IQmathLib.h>
```

## 6.Converting an IQmath Application to Floating-Point

### IQmath application to floating point

1. 在IQMath头文件中，选择Float_Math。头文件将所有IQMATH函数调用转换为其浮点代码格式

2. 在将浮点数写入设备寄存器时，您需要将浮点数转换为整数。同样在从寄存器读取值时，它需要转换为浮动。在这两种情况下，这是通过将数量乘以转换因子来完成的。

   ```c
   For example to convert a floating-point number to IQ15, multiply by 32768.0.   //将float转化为IO15 需要乘以32769.0
   #if MATH_TYPE == IQ_MATH
   PwmReg = (int16)_IQtoIQ15(Var1);
   #else // MATH_TYPE is FLOAT_MATH
   PwmReg = (int16)(32768.0L*Var1);
   #endif
   ```

   # 三、Function Summary

   

   ###  Format conversion utilities : atoIQ, IQtoF, IQtoIQN etc.    //格式转化

   ###  Arithmetic Functions : IQmpy, IQdiv etc.             					//算数函数

   ###  Trigonometric Functions : IQsin, IQcos, IQatan2 etc.       	//三角函数

   ###  Mathematical functions : IQsqrt, IQisqrt etc.                               //数学函数

   ###  Miscellaneous : IQabs, IQsat etc									            //其他
   
   [详细文档](https://www.ti.com.cn/cn/lit/sw/sprc990/sprc990.pdf?ts=1629795754578&ref_url=https%253A%252F%252Fwww.ti.com.cn%252Fsitesearch%252Fdocs%252Funiversalsearch.tsp%253FlangPref%253Dzh-CN%2526searchTerm%253Diqmath%2526nr%253D3376)


# 总结	
<font color=#999AAA > 对于IQMATH的简单介绍，具体使用还请看手册Example。使用难度不大需要把头文件引入让后按照IQ格式区替换所需的代码即可

单还是需要注意一些用法的限制float的大小范围等



