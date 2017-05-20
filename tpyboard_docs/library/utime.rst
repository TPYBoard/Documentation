:mod:`utime` -- 时间相关函数
======================================

.. module:: utime
   :synopsis: 时间相关函数

``utime`` 模块 提供获取当前时间和日期、测量时间间隔和延迟的功能。

**初始时刻**: Unix 使用 POSIX 系统标准，从 1970-01-01 00:00:00 UTC开始。
嵌入式程序从 2000-01-01 00:00:00 UTC 开始。

**保持实际日历日期/时间**:需要一个实时时钟 (RTC)。在底层系统 (包括一些 RTOS 中)，RTC 已经包含在其中。设置时间是通过 OS/RTOS 而不是 MicroPython 完成，查询日期/时间也需要通过系统 API。对于裸板系统时钟依赖于 ``machine.RTC()`` 对象。设置时间通过 ``machine.RTC().datetime(tuple)`` 函数，并通过下面方式维持:

* 后备电池 (可能是选件、扩展板等)。
* 使用网络时间协议 (需要用户设置)。
* 每次上电时手工设置 (大部分只是在硬复位时需要设置，少部分每次复位都需要设置)。

如果实际时间不是通过系统/MicroPython RTC维持，那么下面函数结果可能不是和预期的相同。

函数
---------

.. function:: localtime([secs])

   从初始时间的秒转换为元组: (年, 月, 日, 时, 分, 秒, 星期, yearday) 。如果 secs 是空或者 None，那么使用当前时间。

   * year 年份包括世纪（例如2014）。
   * month   is 1-12
   * mday    is 1-31
   * hour    is 0-23
   * minute  is 0-59
   * second  is 0-59
   * weekday is 0-6 for Mon-Sun
   * yearday is 1-366

.. function:: mktime()

   时间的反函数，它的参数是完整8参数的元组，返回值一个整数自2000年1月1日以来的秒数。

.. function:: sleep(seconds)

   休眠指定的时间（秒），Seconds 可以是浮点数。注意有些版本的 MicroPython 不支持浮点数，为了兼容可以使用 ``sleep_ms()`` 和 ``sleep_us()``函数。

.. function:: sleep_ms(ms)

   延时指定毫秒，参数不能小于0。

.. function:: sleep_us(us)

   延时指定微秒，参数不能小于0。

.. function:: ticks_ms()

    返回不断递增的毫秒计数器，在某些值后会重新计数(未指定)。计数值本身无特定意义，只适合用在``ticks_diff()``。
    
    注：执行标准数学运算（+，-）或关系运算符（<，>，>，> =）直接在这些值上会导致无效结果。执行数学运算然后传递结果作为论据来` ` ticks_diff() ` `或` ` ticks_add() ` `也将导致后一个函数的无效结果。

.. function:: ticks_us()

   和上面``ticks_ms()``类似，只是返回微秒。

.. function:: ticks_cpu()

   与 ``ticks_ms()`` 和 ``ticks_us()`` 类似，具有更高精度 (使用 CPU 时钟)。

   可用性：并非每个端口都实现此功能。


.. function:: ticks_add(ticks, delta)

   Offset ticks value by a given number, which can be either positive or negative.
   Given a ``ticks`` value, this function allows to calculate ticks value ``delta``
   ticks before or after it, following modular-arithmetic definition of tick values
   (see ``ticks_ms()`` above). ``ticks`` parameter must be a direct result of call
   to ``ticks_ms()``, ``ticks_us()``, or ``ticks_cpu()`` functions (or from previous
   call to ``ticks_add()``). However, ``delta`` can be an arbitrary integer number
   or numeric expression. ``ticks_add()`` is useful for calculating deadlines for
   events/tasks. (Note: you must use ``ticks_diff()`` function to work with
   deadlines.)

   Examples::

        # Find out what ticks value there was 100ms ago
        print(ticks_add(time.ticks_ms(), -100))

        # Calculate deadline for operation and test for it
        deadline = ticks_add(time.ticks_ms(), 200)
        while ticks_diff(deadline, time.ticks_ms()) > 0:
            do_a_little_of_something()

        # Find out TICKS_MAX used by this port
        print(ticks_add(0, -1))


.. function:: ticks_diff(ticks1, ticks2)

   Measure ticks difference between values returned from ``ticks_ms()``, ``ticks_us()``,
   or ``ticks_cpu()`` functions. The argument order is the same as for subtraction
   operator, ``ticks_diff(ticks1, ticks2)`` has the same meaning as ``ticks1 - ticks2``.
   However, values returned by ``ticks_ms()``, etc. functions may wrap around, so
   directly using subtraction on them will produce incorrect result. That is why
   ``ticks_diff()`` is needed, it implements modular (or more specifically, ring)
   arithmetics to produce correct result even for wrap-around values (as long as they not
   too distant inbetween, see below). The function returns **signed** value in the range
   [``-TICKS_PERIOD/2`` .. ``TICKS_PERIOD/2-1``] (that's a typical range definition for
   two's-complement signed binary integers). If the result is negative, it means that
   ``ticks1`` occured earlier in time than ``ticks2``. Otherwise, it means that
   ``ticks1`` occured after ``ticks2``. This holds ``only`` if ``ticks1`` and ``ticks2``
   are apart from each other for no more than ``TICKS_PERIOD/2-1`` ticks. If that does
   not hold, incorrect result will be returned. Specifically, if two tick values are
   apart for ``TICKS_PERIOD/2-1`` ticks, that value will be returned by the function.
   However, if ``TICKS_PERIOD/2`` of real-time ticks has passed between them, the
   function will return ``-TICKS_PERIOD/2`` instead, i.e. result value will wrap around
   to the negative range of possible values.

   Informal rationale of the constraints above: Suppose you are locked in a room with no
   means to monitor passing of time except a standard 12-notch clock. Then if you look at
   dial-plate now, and don't look again for another 13 hours (e.g., if you fall for a
   long sleep), then once you finally look again, it may seem to you that only 1 hour
   has passed. To avoid this mistake, just look at the clock regularly. Your application
   should do the same. "Too long sleep" metaphor also maps directly to application
   behavior: don't let your application run any single task for too long. Run tasks
   in steps, and do time-keeping inbetween.

   ``ticks_diff()`` is designed to accommodate various usage patterns, among them:

   Polling with timeout. In this case, the order of events is known, and you will deal
   only with positive results of ``ticks_diff()``::

        # Wait for GPIO pin to be asserted, but at most 500us
        start = time.ticks_us()
        while pin.value() == 0:
            if time.ticks_diff(time.ticks_us(), start) > 500:
                raise TimeoutError

   Scheduling events. In this case, ``ticks_diff()`` result may be negative
   if an event is overdue::

        # This code snippet is not optimized
        now = time.ticks_ms()
        scheduled_time = task.scheduled_time()
        if ticks_diff(now, scheduled_time) > 0:
            print("Too early, let's nap")
            sleep_ms(ticks_diff(now, scheduled_time))
            task.run()
        elif ticks_diff(now, scheduled_time) == 0:
            print("Right at time!")
            task.run()
        elif ticks_diff(now, scheduled_time) < 0:
            print("Oops, running late, tell task to run faster!")
            task.run(run_faster=true)

   Note: Do not pass ``time()`` values to ``ticks_diff()``, you should use
   normal mathematical operations on them. But note that ``time()`` may (and will)
   also overflow. This is known as https://en.wikipedia.org/wiki/Year_2038_problem .


.. function:: time()

   Returns the number of seconds, as an integer, since the Epoch, assuming that
   underlying RTC is set and maintained as described above. If an RTC is not set, this
   function returns number of seconds since a port-specific reference point in time (for
   embedded boards without a battery-backed RTC, usually since power up or reset). If you
   want to develop portable MicroPython application, you should not rely on this function
   to provide higher than second precision. If you need higher precision, use
   ``ticks_ms()`` and ``ticks_us()`` functions, if you need calendar time,
   ``localtime()`` without an argument is a better choice.

   .. admonition:: Difference to CPython
      :class: attention

      In CPython, this function returns number of
      seconds since Unix epoch, 1970-01-01 00:00 UTC, as a floating-point,
      usually having microsecond precision. With MicroPython, only Unix port
      uses the same Epoch, and if floating-point precision allows,
      returns sub-second precision. Embedded hardware usually doesn't have
      floating-point precision to represent both long time ranges and subsecond
      precision, so they use integer value with second precision. Some embedded
      hardware also lacks battery-powered RTC, so returns number of seconds
      since last power-up or from other relative, hardware-specific point
      (e.g. reset).
