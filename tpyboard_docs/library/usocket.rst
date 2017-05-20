*******************************
:mod:`usocket` -- 套接字模块
*******************************

.. module:: usocket
   :synopsis: 套接字模块

提供的BSD套接字接口。

参考对应的`CPython 模块 <https://docs.python.org/3/library/socket.html>`_进行比较。

.. admonition:: Difference to CPython
   :class: attention

   CPython used to have a ``socket.error`` exception which is now deprecated,
   and is an alias of OSError. In MicroPython, use OSError directly.

.. admonition:: Difference to CPython
   :class: attention

   For efficiency and consistency, socket objects in MicroPython implement a stream
   (file-like) interface directly. In CPython, you need to convert a socket to
   a file-like object using ``makefile()`` method. This method is still supported
   by MicroPython (but is a no-op), so where compatibility with CPython matters,
   be sure to use it.

Socket 地址格式
------------------------

下面函数使用 `(ipv4_address, port)` 网络地址, `ipv4_address` 是由点和数字组成的字符串，如 ``"8.8.8.8"``，端口是 1-65535 的数字。注意不能使用域名做为 `ipv4_address`，域名需要先用 ``socket.getaddrinfo()`` 进行解析。

函数
---------

.. function:: socket.socket(socket.AF_INET, socket.SOCK_STREAM, socket.IPPROTO_TCP)

   创建新的套接字，使用指定的地址、类型和协议号。

.. function:: socket.getaddrinfo(host, port)

   传递 主机/端口 到一个5个数据的元组。元组列表的结构如下::

      (family, type, proto, canonname, sockaddr)

   下面显示了怎样连接到一个网址::

      s = socket.socket()
      s.connect(socket.getaddrinfo('www.micropython.org', 80)[0][-1])

   .. admonition:: Difference to CPython
      :class: attention

      CPython raises a ``socket.gaierror`` exception (OSError subclass) in case
      of error in this function. MicroPython doesn't have ``socket.gaierror``
      and raises OSError directly. Note that error numbers of ``getaddrinfo()``
      form a separate namespace and may not match error numbers from
      ``uerrno`` module. To distinguish ``getaddrinfo()`` errors, they are
      represented by negative numbers, whereas standard system errors are
      positive numbers (error numbers are accessible using ``e.args[0]`` property
      from an exception object). The use of negative values is a provisional
      detail which may change in the future.

常数
---------

.. data:: socket.AF_INET
          socket.AF_INET6

   TCP类型定义，可用性取决于特定的开发板。

.. data:: socket.SOCK_STREAM
          socket.SOCK_DGRAM

   Socket类型.

.. data:: socket.IPPROTO_UDP
          socket.IPPROTO_TCP

   IP协议号。

.. data:: socket.SOL_*

   Socket option levels (an argument to ``setsockopt()``). The exact inventory depends on a board.

.. data:: socket.SO_*

   Socket options (an argument to ``setsockopt()``). The exact inventory depends on a board.

Constants specific to WiPy:

.. data:: socket.IPPROTO_SEC

    Special protocol value to create SSL-compatible socket.

class socket
============

方法
-------

.. method:: socket.close

   关闭套接字。一旦关闭后，套接字所有的功能都将失效。远端将接收不到任何数据 (清理队列数据后)。
   在回收垃圾时套接字会自动关闭，但还是推荐在必要时用 close() 去关闭，或, or to use a with statement around them。

.. method:: socket.bind(address)

   将套接字绑定到地址，套接字不能是已经绑定的。

.. method:: socket.listen([backlog])

   允许服务器接收连接。如果指定了 backlog，它不能小于0 (如果小于0将自动设置为0)；超出后系统将拒绝新的连接。如果没有指定，将使用默认值。

.. method:: socket.accept()

   接收连接。套接字需要指定地址并监听连接。返回值是 (conn, address)，其中conn是用来接收和发送数据的套接字，address是绑定到另一端的套接字。

.. method:: socket.connect(address)

   连接到指定地址的远端套接字。

.. method:: socket.send(bytes)

   发送数据。套接字需要已连接到远程。

.. method:: socket.sendall(bytes)

   发送数据。套接字已连接到远程。
   Unlike ``send()``, this method will try to send all of data, by sending data
   chunk by chunk consecutively.

   The behavior of this method on non-blocking sockets is undefined. Due to this,
   on MicroPython, it's recommended to use ``write()`` method instead, which
   has the same "no short writes" policy for blocking sockets, and will return
   number of bytes sent on non-blocking sockets.

.. method:: socket.recv(bufsize)

   接收数据，返回值是数据字节对象。bufsize是接收数据的最大数量。

.. method:: socket.sendto(bytes, address)

   发送数据。套接字没有连接到远程，目标套接字由地址参数指定。

.. method:: socket.recvfrom(bufsize)

  接收数据。返回值是 (bytes, address)，其中 bytes 是字节对象，address 是发送数据的套接字。

.. method:: socket.setsockopt(level, optname, value)

   设置套接字参数。需要的符号常数定义在套接字模块 (SO_* 等)。value 可以是整数或字节对象。

.. method:: socket.settimeout(value)

   设置阻塞套接字超时时间。value 参数可以是代表秒的正浮点数或 None。如果设定大于 0 的参数，在后面套接字操作超出指定时间后将引起 timeout 异常。如果参数是 0，套接字将使用非阻塞模式。如果是 None，套接字使用阻塞模式。

   .. admonition:: Difference to CPython
      :class: attention

      CPython raises a ``socket.timeout`` exception in case of timeout,
      which is an ``OSError`` subclass. MicroPython raises an OSError directly
      instead. If you use ``except OSError:`` to catch the exception,
      your code will work both in MicroPython and CPython.

.. method:: socket.setblocking(flag)

   设置阻塞或非阻塞模式: 如果 flag 是 false，设置非阻塞模式。

   This method is a shorthand for certain ``settimeout()`` calls:

   * ``sock.setblocking(True)`` is equivalent to ``sock.settimeout(None)``
   * ``sock.setblocking(False)`` is equivalent to ``sock.settimeout(0)``

.. method:: socket.makefile(mode='rb', buffering=0)

   Return a file object associated with the socket. The exact returned type depends on the arguments
   given to makefile(). The support is limited to binary modes only ('rb', 'wb', and 'rwb').
   CPython's arguments: ``encoding``, ``errors`` and ``newline`` are not supported.

   .. admonition:: Difference to CPython
      :class: attention

      As MicroPython doesn't support buffered streams, values of ``buffering``
      parameter is ignored and treated as if it was 0 (unbuffered).

   .. admonition:: Difference to CPython
      :class: attention

      Closing the file object returned by makefile() WILL close the
      original socket as well.

.. method:: socket.read([size])

   Read up to size bytes from the socket. Return a bytes object. If ``size`` is not given, it
   reads all data available from the socket until ``EOF``; as such the method will not return until
   the socket is closed. This function tries to read as much data as
   requested (no "short reads"). This may be not possible with
   non-blocking socket though, and then less data will be returned.

.. method:: socket.readinto(buf[, nbytes])

   Read bytes into the ``buf``.  If ``nbytes`` is specified then read at most
   that many bytes.  Otherwise, read at most ``len(buf)`` bytes. Just as
   ``read()``, this method follows "no short reads" policy.

   Return value: number of bytes read and stored into ``buf``.

.. method:: socket.readline()

   读取一行，以换行符结束。
   返回读取的数据行。

.. method:: socket.write(buf)

   Write the buffer of bytes to the socket. This function will try to
   write all data to a socket (no "short writes"). This may be not possible
   with a non-blocking socket though, and returned value will be less than
   the length of ``buf``.

   Return value: number of bytes written.
