如果1个switch有多个case，或是有多个if-else语句，不利于程序的阅读和扩展。改进方法可以使用函数指针数组， 有人也叫表驱动法。

------



## switch

我们先来看看1个switch的例程：
```c
typedef enum {
    E_IDLE = 0,
    E_SYNC,
    E_DETECT_FAULT,
    E_FEEDBACK_FAULT
} Enum_State;

static Enum_State eState = E_IDLE;

static void APP_Idle(void); /* 省略了函数具体实现的功能 */
static void APP_Sync(void);
static void APP_DetectLedFault(void);
static void APP_FeedbackFault(void);

void main(void)
{
	for(;;)
	{
    	switch(eState)
    	{
         case E_IDLE:
        {
            APP_Idle(); 
            break;
        }
        case E_SYNC:
        {
            APP_Sync();
            break;
        }
        case E_DETECT_FAULT:
        {
            APP_DetectLedFault(); 
            break;
        }
        case E_FEEDBACK_FAULT:
        {
            APP_FeedbackFault(); 
            break;
        }
        default: /* E_IDLE */
        {
            break;
        }        
    }
}


```



代码长长的，不好看呢。要是要增加一个新的状态，还得进到这个长长中的代码添加相应的状态程序。

------



## 函数指针数组

我们直接上代码：

```c
typedef enum {
    E_IDLE = 0,
    E_SYNC,
    E_DETECT_FAULT,
    E_FEEDBACK_FAULT
} Enum_State;

static Enum_State eState = E_IDLE;

static void APP_Idle(void); /* 省略了函数具体实现的功能 */
static void APP_Sync(void);
static void APP_DetectLedFault(void);
static void APP_FeedbackFault(void);
void (*APP_Operation[])(void) = { /* Sequence must be consistent with EnumState */
	APP_Idle,
    APP_Sync,
    APP_DetectLedFault,
    APP_FeedbackFault
};

void main(void)
{
	for(;;)
	{
		  APP_Operation[eState]();
	}
}


看了代码，是不是觉得使用函数指针数组简洁了许多呢~~ 如果添加新的状态，直接在函数指针数组APP_Operation[]添加即可，不用修改main()函数。

一般来说，我们常用数组来存储数据的，如 int a[4] = {1, 2, 3, 4} , 则a[0] ~ a[3] 分别存入了整型数据 1, 2, 3, 4。这里解释一下void (*APP_Operation[])(void) = { APP_Idle, APP_Sync, APP_DetectLedFault, APP_FeedbackFault };。函数指针数组，顾名思义，就是这个数组的元素存放的是函数指针（也就是函数名称），因此：

APP_Operation[0]表示的是函数 APP_Idle, APP_Operation[0]()相当就是调用了函数APP_Idle()
APP_Operation[1]表示的是函数 APP_Sync, APP_Operation[1]()相当就是调用了函数APP_Sync()
APP_Operation[2]表示的是函数 APP_DetectLedFault, APP_Operation[2]()相当就是调用了函数APP_DetectLedFault()
APP_Operation[3]表示的是函数 APP_FeedbackFault，APP_Operation[3]()相当就是调用了函数APP_FeedbackFault()

注：其中前面的 void 表示函数的返回值， (void) 表示这些函数的参数类型

```



## 小结

- 如果有大量的switch-case或if-else的情形，请考虑一下函数指针数组，具有解耦效果
- 使用函数指针数组时，要特别注意函数的顺序和数组越界问题。需特别注意