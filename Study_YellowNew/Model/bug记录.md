# 项目场景：

<font color=#999AAA >提示：这里简述项目相关背景：
例如：项目场景：示例:通过蓝牙芯片(HC-05)与手机 APP 通信，每隔 5s 传输一批传感器数据(不是很大)</font>
---


# 问题描述：

<font color=#999AAA >提示：这里描述项目中遇到的问题：
例如：数据传输过程中数据不时出现丢失的情况，偶尔会丢失一部分数据
APP 中接收数据代码：

```c
@Override
        public void run() {
            bytes = mmInStream.read(buffer);
            mHandler.obtainMessage(READ_DATA, bytes, -1, buffer).sendToTarget();
        }
```


 </font>

---


# 原因分析：

<font color=#999AAA >提示：这里填写问题的分析：
例如：Handler 发送消息有两种方式，分别是 Handler.obtainMessage()和 Handler.sendMessage()，其中 obtainMessage 方式当数据量过大时，由于 MessageQuene 大小也有限，所以当 message 处理不及时时，会造成先传的数据被覆盖，进而导致数据丢失。</font>

---


# 解决方案：

<font color=#999AAA >提示：这里填写该问题的具体解决方案：
例如：新建一个 Message 对象，并将读取到的数据存入 Message，然后 mHandler.obtainMessage(READ_DATA, bytes, -1, buffer).sendToTarget();换成 mHandler.sendMessage()。