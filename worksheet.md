# 物理计算入门

## GPIO 引脚

7树莓派强大的特性之一的呢就是板子上面边缘的两行GPIO引脚。GPIO是通用输入输出的缩写。这些针脚是树莓派与外部世界的物理接口。最简单的说就是你可以把这些针脚当做你能够打开或者关闭的用来输入的开关或者Pi本身可以打开关闭的输出开关。


GPIO引脚使得树莓派通过连接电路来控制和检测外部世界。树莓派能够控制多个LED灯，控制lED灯的打开关闭，运行电机，以及其他的事情。还能够检测一个按钮是否被按下，检测温度和光照强度。我们把这些操作称之为物理计算。


树莓派有40个引脚(早期版本有26个引脚)，提供了多种多样的功能。

如果你有一个树莓派引脚标签RasPiO，能够帮助你明确每个针脚的用途。确保面对USB接口的时候你的针脚标签的孔是在外侧的。


![](images/raspio-ports.jpg)

如果你没有引脚标签的话，下面的示意图可以帮助你确定引脚编号

![](images/pinout.png)

你可以看到引脚有3V3，5V，GND，GP2，GP3等标签：


|   |   |   |
|---|---|---|
| 3V3 | 3.3V | 链接到这个引脚可以得到3.3V电压 |
| 5V | 5V | 连接到这个引脚可以得到5V电压 |
| GND | 接地 | 0V接地用来完成电路 |
| GP2 | GPIO 针脚 2 | 这些针脚是通用针脚，可以配置为输入或者输出针脚 |
| ID_SC/ID_SD/DNC | 特殊用途引脚
 ||

**警告**: 如果你严格按照本文教程来，玩耍GPIO引脚将会是一个好玩并且有趣的过程。但是如果你随便连接电线，可能会毁掉树莓派，尤其是你使用5V引脚的时候。当你把大负载的元件连接到树莓派的时候可能会发生不愉快的事情，LED灯是没有问题的，电机可能会有问题。如果你担心损坏树莓派，可以考虑使用诸如[Explorer HAT](https://shop.pimoroni.com/products/explorer-hat)扩展板，直到你能够自信的熟练直接使用GPIO。这个扩展板有电路保护功能防止烧毁树莓派。也可以利用检测开关功能做一个电流急急棒。

## 点亮LED灯

LED等是很容易被烧坏的小元件。如果电流过大就会炸裂，有时候会让你大吃一惊。为了限制流经LED的电流，你应该在电路里串联一个电阻。突然想到很想买PM2.5传感器和甲醛传感器。

把LED灯长的针脚接到3V3这个引脚，然后把短针脚接到GND引脚。电阻可以是大于50Ω的任意电阻。

![](images/led-3v3.png)

这样的话LED就会亮起来，会一直亮，因为LED灯链接的3V3接口会一直是打开的。


现在尝试把LED灯长的针脚从3V3接口重新连接到17号GPIO引脚。
![](images/led-gpio17.png)

LED灯现在就灭了，因为现在连接到了可以用代码控制通断的GPIO接口

## 控制LED亮暗

>Pi Zero比Arduino便宜，所以以后中学阶段可以用Pi Zero做机器人。

GPIO Zero是一个让我们可以用一种简单的方式来控制常见的GPIO元件的新的Python代码库。Raspbian默认安装了GPIO Zero代码库。

1. 从主菜单打开IDLE(`菜单`>`编程`>`Python 3 (IDLE)`.

1.你可以通过直接在Python解释器里输入命令来控制LED的亮暗。(或者说Python **shell**). Let's do this by first importing the GPIO Zero library.在控制LED等之前首先导入GPIO Zero库。你还需要告诉树莓派你正在用哪个针脚-这个例子用的是17号针脚。紧接着大于号 `>>>`, 输入
    
    ```python
	from gpiozero import LED
	led = LED(17)

	```
	敲 **回车**.

1. 为了让LED亮起来，输入下面代码并敲 **Enter**:

	```python
	led.on()
	```

1. 让LED灭可以输入:

	```python
	led.off()
	```

1. Your LED should switch on and then off again. But that's not all you can do.LED就会先亮后灭，但是你能做的不止于此。

## 让LED闪烁

通过使用`time`库，和一个简单的循环，我们可以让LED不断闪烁。	

1. 单击 **文件 > 新建文件**创建新的Python文件.

1. 单击 **文件 > 保存**保存文件为`gpio_led.py`.

1. 输入以下代码:

    ```python
    from gpiozero import LED
    from time import sleep

    led = LED(17)

    while True:
        led.on()
        sleep(1)
        led.off()
        sleep(1)
    ```

1. 按 **Ctrl + S** 保存文件并按 **F5** 执行代码.

1. LED灯开始闪烁. 按 **Ctrl + C** 退出程序.

## 利用按钮获取输入

现在你可以控制输出元件(一个LED)，现在让我们连接并控制一个输入元件：按钮。


1. 按图把一个按钮连接到另外一个GND引脚和2号GPIO引脚:

    ![](images/button.png)

1. 点击 **File > New file**新建文件.

1. 点击 **File > Save**. 把新建文件保存为 `gpio_button.py`.

1. 现在你需要使用`Button`类，并且告诉该类按钮连接到了2号引脚。输入以下代码到你新建的文件：

	```python
	from gpiozero import Button
	button = Button(2)
	```

1. 现在当按钮按下的时候你的程序就可以做一些事情。添加以下代码:

	```python
	button.wait_for_press()
	print('你按我了')
	```
1. 按 **Ctrl + S** 保存代码并按 **F5**执行代码. 
1. 单击按钮文本就会出现. 

## 手动控制LED

You can now combine your two programs written so far to control the LED using the button.现在可以目前为止写的两个程序结合起来通过按钮来控制LED。

1. 单击 **File > New file**新建文件.

1. 单击 **File > Save**保存文件为 `gpio_control.py`.

1. 输入以下代码:

    ```python
    from gpiozero import LED, Button
    from time import sleep
    
    led = LED(17)
    button = Button(2)
    
    button.wait_for_press()
    led.on()
    sleep(3)
    led.off()
    ```
	
1. 保存并运行程序。当你按按钮的时候LED持续亮3秒然后灭掉。

## 制作一个开关

通过开关，按下和松开按钮一次就会点亮LED，再次按下和松开就会关掉LED。

1. 修改程序如下:

	```python
	from gpiozero import LED, Button
	from time import sleep

	led = LED(17)
	button = Button(2)

	while True:
		button.wait_for_press()
		led.toggle()
	```

    `led.toggle()` 把LED的状态从亮改为灭，从灭改为亮。因为在循环内调用函数，所以每次按按钮的是LED就会点亮或者关闭。

1. These don't block the flow of the program, so if they are placed in a loop, the program will continue to cycle indefinitely.
如果只有按钮按下的是LED才会亮是一个很好的注意。因为有了GPIO Zero这个变得很简单。`Button`类有两个方法分别是`when_pressed` and `when_released`。这两个方法不会阻塞程序的流程，所以即便你把这两个函数放到一个循环里，程序仍然会无限循环。

1. 修改代码如下:

    ```python
    from gpiozero import LED, Button
    from signal import pause

    led = LED(17)
    button = Button(2)

    button.when_pressed = led.on
    button.when_released = led.off

    pause()
    ```

>这种写法很明显比用Arduino要容易的多，电路也简单的多，充分的体现了Python语言简洁优雅，开发效率高的特点

1. Save and run the program. Now when the button is pressed, the LED will light up. It will turn off again when the button is released.保持并运行程序，现在按下按钮，LED亮；松开按钮，LED灭。

## 接下来学习啥?

通过树莓派你可以控制和监测大量其他的元件。看看下面的列表，就会明白这很容易。

- [控制蜂鸣器](buzzer.md)  
- [制作交通灯](trafficlights.md)  
- [光敏电阻的使用](ldr.md)  
- [人体红外传感器的使用](pir.md)  
- [超声波距离传感器的使用](distance.md)
- [模拟输入](analogue.md)
- [使用电机](motors.md)
