:mod:`uheapq` -- 堆排序算法
=====================================

.. module:: uheapq
   :synopsis: 堆排序算法

提供了堆排序算法。

堆队列是一个列表，它的元素以特定的方式存储。

函数
---------

.. function:: heappush(heap, item)

   把 ``item`` 推到 ``heap``。

.. function:: heappop(heap)

   从 ``heap``弹出第一个元素并返回。  如果是堆时空的会抛出IndexError。

.. function:: heapify(x)

   将列表 ``x`` 转换成堆。
