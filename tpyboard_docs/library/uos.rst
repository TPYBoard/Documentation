:mod:`uos` -- 基本的 "操作系统" 服务
===============================================

.. module:: uos
   :synopsis: 基本的 "操作系统" 服务

``os`` 模块包含用于文件系统访问和``urandom``功能。

端口详解
--------------

``/``文件系统的根目录和可用的物理驱动器可以从这里访问。
如下:

    ``/flash``      -- 内部闪存文件系统

    ``/sd``         -- SD卡（如果插入SD卡）

.. only:: port_pyboard

    正常启动后, 当前目录是 ``/flash`` 如果没有插入SD卡,否则目录是 ``/sd``。

.. only:: port_wipy

    正常启动后, 当前目录是 ``/flash``。

函数
---------

.. function:: chdir(path)

   更改当前目录。

.. function:: getcwd()

   获取当前目录。

.. function:: ilistdir([dir])

   这个函数返回一个迭代器，然后产生三元组对应正在列出的目录中的条目。没有参数，它列出了
当前目录，否则它列出了目录给出的`dir`。

   3-元组的形式`(name, type, inode)`:

    - `name` 是一个字符串（或字节，如果是一个字节对象），是输入的名称；
    - `type` 是一个整数，指定的条目类型，与普通文件和目录0x4000 0x8000；
    - `inode` 对应文件的inode的整数，可0的文件系统，没有这样的概念。

.. function:: listdir([dir])

   没有参数，列出当前目录。否则列出给定目录。

.. function:: mkdir(path)

   创建一个目录。

.. function:: remove(path)

   删除文件。

.. function:: rmdir(path)

   删除目录。

.. function:: rename(old_path, new_path)

   重命名文件。

.. function:: stat(path)

   获取文件或目录的状态。

.. function:: statvfs(path)

   得到一个文件系统的状态。

   按下列顺序返回带有文件系统信息的元组:

        * ``f_bsize`` -- file system block size
        * ``f_frsize`` -- fragment size
        * ``f_blocks`` -- size of fs in f_frsize units
        * ``f_bfree`` -- number of free blocks
        * ``f_bavail`` -- number of free blocks for unpriviliged users
        * ``f_files`` -- number of inodes
        * ``f_ffree`` -- number of free inodes
        * ``f_favail`` -- number of free inodes for unpriviliged users
        * ``f_flag`` -- mount flags
        * ``f_namemax`` -- maximum filename length

   Parameters related to inodes: ``f_files``, ``f_ffree``, ``f_avail``
   and the ``f_flags`` parameter may return ``0`` as they can be unavailable
   in a port-specific implementation.

.. function:: sync()

   同步所有的文件系统。

.. function:: urandom(n)

   返回带有n个随机字节的字节对象。它由硬件随机数生成器生成。

.. only:: port_wipy

    .. function:: mount(block_device, mount_point, \*, readonly=False)

       Mounts a block device (like an ``SD`` object) in the specified mount
       point. Example::

          os.mount(sd, '/sd')

    .. function:: unmount(path)

       Unmounts a previously mounted block device from the given path.

    .. function:: mkfs(block_device or path)

       Formats the specified path, must be either ``/flash`` or ``/sd``.
       A block device can also be passed like an ``SD`` object before
       being mounted.

    .. function:: dupterm(stream_object)

       Duplicate the terminal (the REPL) on the passed stream-like object.
       The given object must at least implement the ``.read()`` and ``.write()`` methods.
