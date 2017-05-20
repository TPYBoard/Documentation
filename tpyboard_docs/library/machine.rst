:mod:`machine` --- 与硬件相关的功能
====================================================

.. module:: machine
   :synopsis: 与硬件相关的功能

``machine`` 模块包含与特定板上的硬件相关的特定函数。
在这个模块中的大多数功能允许实现直接和不受限制地访问和控制系统上的硬件块（如CPU，定时器，总线等）。
使用不当，会导致故障，死机，你会崩溃，在极端的情况下，硬件损坏。

.. _machine_callbacks:

A note of callbacks used by functions and class methods of ``machine`` module:
all these callbacks should be considered as executing in an interrupt context.
This is true for both physical devices with IDs >= 0 and "virtual" devices
with negative IDs like -1 (these "virtual" devices are still thin shims on
top of real hardware and real hardware interrupts). See :ref:`isr_rules`.

复位功能
-----------------------

.. function:: reset()

   重置设备的方式类似类似按下rst按钮。

.. function:: reset_cause()

   Get the reset cause. See :ref:`constants <machine_constants>` for the possible return values.

中断功能
---------------------------

.. function:: disable_irq()

   Disable interrupt requests.
   Returns the previous IRQ state which should be considered an opaque value.
   This return value should be passed to the ``enable_irq`` function to restore
   interrupts to their original state, before ``disable_irq`` was called.

.. function:: enable_irq(state)

   Re-enable interrupt requests.
   The ``state`` parameter should be the value that was returned from the most
   recent call to the ``disable_irq`` function.

有效的相关功能
-----------------------

.. function:: freq()

    Returns CPU frequency in hertz.

.. function:: idle()

   Gates the clock to the CPU, useful to reduce power consumption at any time during
   short or long periods. Peripherals continue working and execution resumes as soon
   as any interrupt is triggered (on many ports this includes system timer
   interrupt occurring at regular intervals on the order of millisecond).

.. function:: sleep()

   Stops the CPU and disables all peripherals except for WLAN. Execution is resumed from
   the point where the sleep was requested. For wake up to actually happen, wake sources
   should be configured first.

.. function:: deepsleep()

   Stops the CPU and all peripherals (including networking interfaces, if any). Execution
   is resumed from the main script, just as with a reset. The reset cause can be checked
   to know that we are coming from ``machine.DEEPSLEEP``. For wake up to actually happen,
   wake sources should be configured first, like ``Pin`` change or ``RTC`` timeout.

.. only:: port_wipy

    .. function:: wake_reason()

        Get the wake reason. See :ref:`constants <machine_constants>` for the possible return values.

更多功能
-----------------------

.. only:: port_wipy

    .. function:: rng()

        Return a 24-bit software generated random number.

.. function:: unique_id()

   Returns a byte string with a unique identifier of a board/SoC. It will vary
   from a board/SoC instance to another, if underlying hardware allows. Length
   varies by hardware (so use substring of a full value if you expect a short
   ID). In some MicroPython ports, ID corresponds to the network MAC address.

.. function:: time_pulse_us(pin, pulse_level, timeout_us=1000000)

   Time a pulse on the given `pin`, and return the duration of the pulse in
   microseconds.  The `pulse_level` argument should be 0 to time a low pulse
   or 1 to time a high pulse.

   If the current input value of the pin is different to `pulse_level`,
   the function first (*) waits until the pin input becomes equal to `pulse_level`,
   then (**) times the duration that the pin is equal to `pulse_level`.
   If the pin is already equal to `pulse_level` then timing starts straight away.

   The function will return -2 if there was timeout waiting for condition marked
   (*) above, and -1 if there was timeout during the main measurement, marked (**)
   above. The timeout is the same for both cases and given by `timeout_us` (which
   is in microseconds).

.. _machine_constants:

常数
---------

.. data:: machine.IDLE
          machine.SLEEP
          machine.DEEPSLEEP

    IRQ wake values.

.. data:: machine.PWRON_RESET
          machine.HARD_RESET
          machine.WDT_RESET
          machine.DEEPSLEEP_RESET
          machine.SOFT_RESET

    Reset causes.

.. data:: machine.WLAN_WAKE
          machine.PIN_WAKE
          machine.RTC_WAKE

    Wake-up reasons.

类
-------

.. only:: not port_wipy

 .. toctree::
   :maxdepth: 1

   machine.I2C.rst
   machine.Pin.rst
   machine.RTC.rst
   machine.SPI.rst
   machine.Timer.rst
   machine.UART.rst
   machine.WDT.rst

.. only:: port_wipy

 .. toctree::
   :maxdepth: 1

   machine.ADC.rst
   machine.I2C.rst
   machine.Pin.rst
   machine.RTC.rst
   machine.SD.rst
   machine.SPI.rst
   machine.Timer.rst
   machine.UART.rst
   machine.WDT.rst
