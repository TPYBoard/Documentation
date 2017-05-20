:mod:`array` -- 数字数据数组
======================================

.. module:: array
   :synopsis: efficient arrays of numeric data

查看更多 `Python 数组 <https://docs.python.org/3/library/array.html>`_ 信息。

支持代码格式: ``b``, ``B``, ``h``, ``H``, ``i``, ``I``, ``l``,
``L``, ``q``, ``Q``, ``f``, ``d`` (后2个支持浮点数)。

类
-------

.. class:: array.array(typecode, [iterable])

    指定类型创建数组元素。用可选项［］做为数组的初始值，可选项［］未指定的，则创建空数组。

    .. method:: append(val)

        将新元素添加到数组的结尾，并将其扩展。

    .. method:: extend(iterable)

        使用迭代方式将新元素添加到数组的结尾，并将其扩展。
