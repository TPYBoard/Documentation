:mod:`sys` -- 系统特有功能函数
=======================================

.. module:: sys
   :synopsis: 系统特有功能函数

函数
---------

.. function:: exit(retval=0)

   终止当前程序给定的退出代码。 函数会抛出 ``SystemExit`` 异常。

.. function:: print_exception(exc, file=sys.stdout)

   打印异常与追踪到一个类似文件的对象 `file` (或者缺省 ``sys.stdout`` ).

   .. admonition:: Difference to CPython
      :class: attention

      This is simplified version of a function which appears in the
      ``traceback`` module in CPython. Unlike ``traceback.print_exception()``,
      this function takes just exception value instead of exception type,
      exception value, and traceback object; `file` argument should be
      positional; further arguments are not supported. CPython-compatible
      ``traceback`` module can be found in micropython-lib.

常数
---------

.. data:: argv

   当前程序启动时参数的可变列表。

.. data:: byteorder

   系统字节顺序 ("little" or "big").

.. data:: implementation

   使用当前Python实现的。对于micropython，它具有以下属性:

   * `name` - string "micropython"
   * `version` - tuple (major, minor, micro), e.g. (1, 7, 0)

   This object is the recommended way to distinguish MicroPython from other
   Python implementations (note that it still may not exist in the very
   minimal ports).

   .. admonition:: Difference to CPython
      :class: attention

      CPython mandates more attributes for this object, but the actual useful
      bare minimum is implemented in MicroPython.

.. data:: maxsize

   一个土生土长的整数类型的最大值可以把握当前的平台，或最大值的micropython整型表示，
   如果它小于平台最大值（即micropython端口没有长整型数据支持的情况下）。

   可用于检测“意义”的一个平台（32位和64位，等）。建议不要直接将此属性与某些值比较，而是计算它的位数::

    bits = 0
    v = sys.maxsize
    while v:
        bits += 1
        v >>= 1
    if bits > 32:
        # 64-bit (or more) platform
        ...
    else:
        # 32-bit (or less) platform
        # Note that on 32-bit platform, value of bits may be less than 32
        # (e.g. 31) due to peculiarities described above, so use "> 16",
        # "> 32", "> 64" style of comparisons.

.. data:: modules

   加载模块字典。在一部分环境中它可能不包含内置模块。

.. data:: path

   搜索导入模块的可变目录列表。

.. data:: platform

   The platform that MicroPython is running on. For OS/RTOS ports, this is
   usually an identifier of the OS, e.g. ``"linux"``. For baremetal ports it
   is an identifier of a board, e.g. "pyboard" for the original MicroPython
   reference board. It thus can be used to distinguish one board from another.
   If you need to check whether your program runs on MicroPython (vs other
   Python implementation), use ``sys.implementation`` instead.

.. data:: stderr

   标准错误流。

.. data:: stdin

   标准输入流。

.. data:: stdout

   标准输出流。

.. data:: version

   符合的Python语言版本，如字符串。

.. data:: version_info

   Python语言版本，实现符合，作为一个元组的值。
