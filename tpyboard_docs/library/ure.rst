:mod:`ure` -- 正则表达式
=================================

.. module:: ure
   :synopsis: 正则表达式

正则表达式用于测试字符串的某个模式，执行正则表达式操作。 
正则表达式支持 CPython 子集 ``re`` 模块 (实际是 POSIX 扩展正则表达式的子集)。

支持操作符:

``'.'``
   匹配任意字符。

``'[]'``
   匹配字符集合，支持单个字符和一个范围。

``'^'``

``'$'``

``'?'``

``'*'``

``'+'``

``'??'``

``'*?'``

``'+?'``

重复计数 (``{m,n}``), 不支持高级的断言、命名组等。


函数
---------

.. function:: compile(regex)

   编译正则表达式，返回 ``regex`` 对象。

.. function:: match(regex, string)

   用 ``string`` 匹配 ``regex``，匹配总是从字符串的开始匹配。

.. function:: search(regex, string)

   在 ``string`` 中搜索 ``regex``。不同于匹配，它搜索第一个匹配位置的正则表达式字符串 (结果可能会是0)。

.. data:: DEBUG

   标志值，显示表达式的调试信息。


Regex 对象
-------------

编译正则表达式，使用``ure.compile()``创建实例。

.. method:: regex.match(string)

.. method:: regex.search(string)

.. method:: regex.split(string, max_split=-1)


匹配对象
-------------

匹配对象是 ``match()`` 和 ``search()`` 方法返回值。

.. method:: match.group([index])

   只支持数字组。
