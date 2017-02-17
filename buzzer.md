# 蜂鸣器

蜂鸣器有两种: *有源蜂鸣器* and *无源蜂鸣器*。
 The *active* buzzers are a lot simpler to use, so these are covered here.
*无源蜂鸣器*输入电压就会发声。需要特定的信号来产生多样的音调。*有源蜂鸣器*更容易使用，介绍如下。

## 连接蜂鸣器

有源蜂鸣器用起来就像LED一样，但是有源蜂鸣器更加稳健，不需要串联电阻来保护有源蜂鸣器：

按照下图连接电路:

![buzzer](images/buzzer-circuit.png)

1. 导入 `Buzzer` to the `from gpiozero import...` 类:

    ```python
    from gpiozero import Buzzer
	from time import sleep
    ```

1. 在你创建按钮和灯后面添加`Buzzer`对象。:

    ```python
    buzzer = Buzzer(17)
    ```

1. 在GPIO Zero库中，`Buzzer`就像一个`LED`，在循环中试着添加`buzzer.on()` and `buzzer.off()`:

    ```python
    while True:
        buzzer.on()
	    sleep(1)
        buzzer.off()
		sleep(1)

    ```

1. A `Buzzer` has a `beep()` method which works like an `LED`'s `blink`. `Buzzer`类有一个方法`beep()`类似于`LED`的`blink`。试试看:

    ```python
    while True:
        buzzer.beep()
    ```

## 拓展延伸?

- 到下一个学习单学习用GPIO Zero打造[交通灯](trafficlights.md) 系统.
