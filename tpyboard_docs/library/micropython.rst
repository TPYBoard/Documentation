:mod:`micropython` -- 访问并控制MicroPython底层
==============================================================

.. module:: micropython
   :synopsis: 访问并控制MicroPython底层

函数
---------

.. function:: alloc_emergency_exception_buf(size)

   为异常缓冲区分配``size``内存字节（大小约为100字节）。
   缓冲区被用来创建异常的情况下，当正常的RAM分配将失败（如在中断处理程序），因此在这些情况下，提供有用的回溯信息。

   使用这个函数的一个好方法是把它放在主脚本的开始（如boot.py或mail.py）然后急救例外缓冲区将积极为所有它之后的代码。

.. function:: mem_info([verbose])

   打印当前使用的内存信息。如果给出了``verbose``参数，则打印额外的信息。

   打印的信息是实现依赖的，但当前包含堆栈和堆的使用量。
   在详细模式中，它打印出整个堆，标示被使用和可用块。

.. function:: qstr_info([verbose])

   目前关于保留的字符串打印信息。如果给出了``verbose``参数，则打印额外的信息。

   The information that is printed is implementation dependent, but currently
   includes the number of interned strings and the amount of RAM they use.  In
   verbose mode it prints out the names of all RAM-interned strings.
