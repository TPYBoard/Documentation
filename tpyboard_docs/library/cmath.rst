:mod:`cmath` -- 复数的数学函数
==========================================================

.. module:: cmath
   :synopsis: 复数的数学函数

``cmath``提供了基本的复数运算功能。它不支持 WiPy 和 ESP8266，因为需要浮点库支持.

函数
---------

.. function:: cos(z)

   返回``z``的余弦。

.. function:: exp(z)

   返回``z``的指数。

.. function:: log(z)

   返回``z``的对数。

.. function:: log10(z)

   返回``z``的常用对数。

.. function:: phase(z)

   返回``z``的相位, 范围是(-pi, +pi]，以弧度表示。

.. function:: polar(z)

   返回``z``的极坐标.

.. function:: rect(r, phi)

   返回`模量`r``和相位``phi``的复数。

.. function:: sin(z)

   返回``z``的正弦。

.. function:: sqrt(z)

   返回``z``的平方根。

常数
---------

.. data:: e

   自然对数的指数。

.. data:: pi

   圆周率。
