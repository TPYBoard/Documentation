:mod:`uio` -- 输入/输出流
==================================

.. module:: uio
   :synopsis:  输入/输出流

包含流类型 (类似文件) 对象和帮助函数。

概念层次
--------------------

.. admonition:: Difference to CPython
   :class: attention

   Conceptual hierarchy of stream base classes is simplified in MicroPython,
   as described in this section.

(Abstract) base stream classes, which serve as a foundation for behavior
of all the concrete classes, adhere to few dichotomies (pair-wise
classifications) in CPython. In MicroPython, they are somewhat simplified
and made implicit to achieve higher efficiencies and save resources.

An important dichotomy in CPython is unbuffered vs buffered streams. In
MicroPython, all streams are currently unbuffered. This is because all
modern OSes, and even many RTOSes and filesystem drivers already perform
buffering on their side. Adding another layer of buffering is counter-
productive (an issue known as "bufferbloat") and takes precious memory.
Note that there still cases where buffering may be useful, so we may
introduce optional buffering support at a later time.

But in CPython, another important dichotomy is tied with "bufferedness" -
it's whether a stream may incur short read/writes or not. A short read
is when a user asks e.g. 10 bytes from a stream, but gets less, similarly
for writes. In CPython, unbuffered streams are automatically short
operation susceptible, while buffered are guarantee against them. The
no short read/writes is an important trait, as it allows to develop
more concise and efficient programs - something which is highly desirable
for MicroPython. So, while MicroPython doesn't support buffered streams,
it still provides for no-short-operations streams. Whether there will
be short operations or not depends on each particular class' needs, but
developers are strongly advised to favor no-short-operations behavior
for the reasons stated above. For example, MicroPython sockets are
guaranteed to avoid short read/writes. Actually, at this time, there is
no example of a short-operations stream class in the core, and one would
be a port-specific class, where such a need is governed by hardware
peculiarities.

The no-short-operations behavior gets tricky in case of non-blocking
streams, blocking vs non-blocking behavior being another CPython dichotomy,
fully supported by MicroPython. Non-blocking streams never wait for
data either to arrive or be written - they read/write whatever possible,
or signal lack of data (or ability to write data). Clearly, this conflicts
with "no-short-operations" policy, and indeed, a case of non-blocking
buffered (and this no-short-ops) streams is convoluted in CPython - in
some places, such combination is prohibited, in some it's undefined or
just not documented, in some cases it raises verbose exceptions. The
matter is much simpler in MicroPython: non-blocking stream are important
for efficient asynchronous operations, so this property prevails on
the "no-short-ops" one. So, while blocking streams will avoid short
reads/writes whenever possible (the only case to get a short read is
if end of file is reached, or in case of error (but errors don't
return short data, but raise exceptions)), non-blocking streams may
produce short data to avoid blocking the operation.

The final dichotomy is binary vs text streams. MicroPython of course
supports these, but while in CPython text streams are inherently
buffered, they aren't in MicroPython. (Indeed, that's one of the cases
for which we may introduce buffering support.)

Note that for efficiency, MicroPython doesn't provide abstract base
classes corresponding to the hierarchy above, and it's not possible
to implement, or subclass, a stream class in pure Python.

函数
---------

.. function:: open(name, mode='r', **kwargs)

    打开一个文件，关联到内建函数``open()``。所有端口 (用于访问文件系统) 需要支持模式参数，但支持其他参数不同的端口。

类
-------

.. class:: FileIO(...)

    这个文件类型用二进制方式打开文件，等于使用``open(name, "rb")``。
    不应直接使用这个实例。

.. class:: TextIOWrapper(...)

    这个类型以文本方式打开文件，等同于使用``open(name, "rt")``不应直接使用这个实例。

.. class:: StringIO([string])
.. class:: BytesIO([string])

    内存文件对象。StringIO 用于文本模式 I/O (用 “t” 打开文件)，BytesIO 用于二进制方式 (用 “b” 方式)。文件对象的初始内容可以用字符串参数指定（stringio 用普通字符串，bytesio用byets对象）。所有的文件方法，如 ``read()``, ``write()``, ``seek()``, ``flush()``,
    ``close()`` 都可以用在这些对象上，包括下面方法:

    .. method:: getvalue()

        获取缓存去内容。
