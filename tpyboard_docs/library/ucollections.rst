:mod:`ucollections` -- 收集和容器类型
=====================================================

.. module:: ucollections
   :synopsis: 收集和容器类型

模块实现先进的集合和容器类型来保存/累积各种对象。

类
-------

.. function:: namedtuple(name, fields)

    这是工厂函数创建一个新的namedtuple型与一个特定的字段名称和集合。
    namedtuple是元组允许子类要访问它的字段不仅是数字索引，而且还具有属性使用符号字段名访问语法。
    字段是字符串序列指定字段名称。为了兼容的实现也可以用空间分隔的字符串命名的字段（但效率较低）
    使用示例::

        from ucollections import namedtuple

        MyTuple = namedtuple("MyTuple", ("id", "name"))
        t1 = MyTuple(1, "foo")
        t2 = MyTuple(2, "bar")
        print(t1.name)
        assert t2.name == t2[1]

.. function:: OrderedDict(...)

    ``dict`` 类型的子类，记住并保留键的追加顺序。keys/items返回的顺序被加入::

        from ucollections import OrderedDict

        # To make benefit of ordered keys, OrderedDict should be initialized
        # from sequence of (key, value) pairs.
        d = OrderedDict([("z", 1), ("a", 2)])
        # More items can be added as usual
        d["w"] = 5
        d["b"] = 3
        for k, v in d.items():
            print(k, v)

    输出::

        z 1
        a 2
        w 5
        b 3
