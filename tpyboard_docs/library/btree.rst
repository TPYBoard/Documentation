:mod:`btree` -- 简单的二叉树数据库
=====================================

.. module:: btree
   :synopsis: 简单的二叉树数据库

``btree``模块实现了一个简单的键值使用外部存储（磁盘文件，或在一般情况下，随机访问流）的数据库。
密钥存储在数据库中，除了由一个关键值的有效检索，数据库还支持有效的有序范围扫描（检索与给定范围内的键的值）。
在应用程序界面，BTree数据库工作接近可能的方式标准字典，一个显著的区别是，键和值必须是字节的对象（所以，如果你想存储其他类型的对象，你需要它们序列化字节第一）。

该模块是基于著名的BerkelyDB类库，版本1.xx。

例子::

    import btree

    # First, we need to open a stream which holds a database
    # This is usually a file, but can be in-memory database
    # using uio.BytesIO, a raw flash section, etc.
    f = open("mydb", "w+b")

    # Now open a database itself
    db = btree.open(f)

    # The keys you add will be sorted internally in the database
    db[b"3"] = b"three"
    db[b"1"] = b"one"
    db[b"2"] = b"two"

    # Prints b'two'
    print(db[b"2"])

    # Iterate over sorted keys in the database, starting from b"2"
    # until the end of the database, returning only values.
    # Mind that arguments passed to values() method are *key* values.
    # Prints:
    #   b'two'
    #   b'three'
    for word in db.values(b"2"):
        print(word)

    del db[b"2"]

    # No longer true, prints False
    print(b"2" in db)

    # Prints:
    #  b"1"
    #  b"3"
    for key in db:
        print(key)

    db.close()

    # Don't forget to close the underlying stream!
    f.close()


函数
---------

.. function:: open(stream, \*, flags=0, cachesize=0, pagesize=0, minkeypage=0)

   从随机访问流（如打开文件）打开数据库。所有其他参数都是可选的关键字，并允许调整数据库操作先进的参数（大多数用户不需要）：

   * `flags` - 当前未使用
   * `cachesize` - 建议的最大内存缓存大小为字节。对于一个使用较大值的内存可以提高性能。这个值只是一个建议，如果可能使用更多的内存模块值设置太低。
   * `pagesize` - 页面大小用于BTree节点。可接受的范围内是512-65536。如果0，将使用底层I/O块大小（内存使用和性能之间的最佳折衷方案）。
   * `minkeypage` - 每页存储关键字的最小数目。默认值0等于2。

   返回一个` 二叉树 `对象，实现一个字典协议（的方法），和一些额外的方法介绍如下。

方法
-------

.. method:: btree.close()

   关闭数据库。在处理结束时关闭数据库是强制性的，因为一些不成文的数据可能仍在缓存中。注意，这不会关闭数据库打开的底层流，它应该分别关闭（这也是强制性的，以确保从缓冲区刷新到基础存储的数据）。

.. method:: btree.flush()

   将缓存中的任何数据写入基础流。

.. method:: btree.__getitem__(key)
.. method:: btree.get(key, default=None)
.. method:: btree.__setitem__(key, val)
.. method:: btree.__detitem__(key)
.. method:: btree.__contains__(key)

   标准字典的方法.

.. method:: btree.__iter__()

   可直接遍历（类似于字典）二叉树对象来获取所有的键值。

.. method:: btree.keys([start_key, [end_key, [flags]]])
.. method:: btree.values([start_key, [end_key, [flags]]])
.. method:: btree.items([start_key, [end_key, [flags]]])

   这些方法与标准字典方法类似，但也可以使用可选参数迭代一个关键子区域，而不是整个数据库。

常数
---------

.. data:: INCL

   `keys()`, `values()`, `items()`方法的标志，用于扫描键值直到结束。

.. data:: DESC

   `keys()`, `values()`, `items()` 方法的标志，用于降序扫描。
