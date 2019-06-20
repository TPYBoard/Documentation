.. _TPYBoardv201-index:

TPYBoard v201 典型实例
=========================================

*TPYBoard v201开发板是在v102基础上增加了以太网的功能，基本使用完全兼容v102的教程，所以这里，我们就直接引用了v102的教程*

- `TPYBoard相关资料(引脚图、尺寸图等) <http://docs.tpyboard.com/zh/latest/tpyboard/hardware/>`_

新手入门
-----------

帮助初学者快速掌握TPYBoard v201开发板的使用技巧。

.. toctree::
   :maxdepth: 1
   :numbered:
        
   ../v10x/start.rst
   ../v10x/repl.rst
   ../v10x/V102 (10).rst
   ../v10x/V102 (13).rst
   ../v10x/V102 (14).rst
   ../v10x/V102 (15).rst
   ../v10x/V102 (17).rst
   ../v10x/V102 (19).rst
   ../v10x/V102 (20).rst
   ../v10x/V102 (21).rst

进阶训练
-----------

学习使用TPYBoard v201开发板进行一些基础的实例训练。

.. toctree::
   :maxdepth: 1
   :numbered:
        
   ../v10x/intro.rst
   ../v10x/metaldetector.rst
   ../v10x/nightlight.rst
   ../v10x/dht11.rst
   ../v10x/88led.rst
   ../v10x/template.rst
   ../v10x/script.rst
   ../v10x/pm2.5.rst
   ../v10x/ultrasonicranging.rst
   ../v10x/diytime.rst
   ../v10x/oled.rst
   ../v10x/1602.rst
   ../v10x/airfan.rst
   ../v10x/servo.rst

高手专场
-----------

经过前面两个阶段的练习，想必你已经成为一名TPYBoard的高手了。那就来挑战下高手专场的实例训练吧！

.. toctree::
   :maxdepth: 1
   :numbered:
   
   set.rst
   cloudadder.rst
   temperaturehumidity.rst

   
**注意：**
 - **v201开发板上并没有板载的三轴加速度传感器。**
 - **以太网模块占用了开发板上的UART 6(TX = Y1, RX = Y2)。**