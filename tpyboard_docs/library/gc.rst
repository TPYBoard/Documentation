:mod:`gc` -- 控制垃圾回收
==========================================

.. module:: gc
   :synopsis: 控制垃圾回收

Functions
---------

.. function:: enable()

   启动自动垃圾回收。

.. function:: disable()

   禁用自动垃圾回收。 堆内存仍然可以分配，垃圾回收仍然可以手动启动使用 :meth:`gc.collect`.

.. function:: collect()

   运行垃圾回收。

.. function:: mem_alloc()

   返回分配的堆RAM的字节数。

.. function:: mem_free()

   返回可用堆内存的字节数。
