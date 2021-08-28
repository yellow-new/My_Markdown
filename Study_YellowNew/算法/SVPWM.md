



# 算法 SVPWM



  

   ## 空间矢量

###  		ABC三个桥臂分别有0,1两种状态，0是下管开通上管关断，1是上管开通下管关断。三个桥臂的两种状态总共有八个组合，产生的结果如下：

​    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628152945167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21pY2hhZWxm,size_16,color_FFFFFF,t_70)



## 扇区判断

### Uref这个矢量按照我们约定的方向在圆内依次运行。在每个扇区内Uref都由两个相邻的矢量根据不同的时间合成矢量，因此第一步我们需要知道Uref在哪个扇区。



```c
U_1= U_β
U_2=  √3/2 U_α-1/2 U_β
U_3= -√3/2 U_α-1/2 U_β
```

![park](C:\My_Project_Markdown\Study_YellowNew\image\park.jpg)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628153725357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21pY2hhZWxm,size_16,color_FFFFFF,t_70)

![svpwm](C:\My_Project_Markdown\Study_YellowNew\image\svpwm.gif)





![img](https://img-blog.csdnimg.cn/img_convert/8dc91f8ccb7a11605a289c6004b45423.gif)
