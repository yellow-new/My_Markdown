
* PID调试一般原则 
    > a.在输出不振荡时，增大比例增益P。 
    >
    > b.在输出不振荡时，减小积分时间常数Ti。 
    >
    > c.在输出不振荡时，增大微分时间常数Td。 
    >
    > （他们三个任何谁过大都会造成系统的震荡。）
    
* 一般步骤

    > a.确定比例增益P ：确定比例增益P 时，首先去掉PID的积分项和微分项，一般是令Ti=0、Td=0（具体见PID的参数设定说明），使PID为纯比例调节。输入设定为系统允许的最大值的60%~70%，由0逐渐加大比例增益P，直至系统出现振荡；再反过来，从此时的比例增益P逐渐减小，直至系统振荡消失，记录此时的比例增益P，设定PID的比例增益P为当前值的60%~70%。比例增益P调试完成。

>     b.确定积分时间常数Ti比例增益P确定后，设定一个较大的积分时间常数Ti的初值，然后逐渐减小Ti，直至系统出现振荡，之后在反过来，逐渐加大Ti，直至系统振荡消失。记录此时的Ti，设定PID的积分时间常数Ti为当前值的150%~180%。积分时间常数Ti调试完成。

>   c.确定积分时间常数Td 积分时间常数Td一般不用设定，为0即可。若要设定，与确定 P和Ti的方法相同，取不振荡时的30%。
 >  d.系统空载、带载联调，再对PID参数进行微调，直至满足要求：理想时间两个波，前高后低4比

