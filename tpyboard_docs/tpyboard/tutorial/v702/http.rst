[Micropython]TPYBoard v702 HTTP应用功能
=============================================

版权声明：翻译整理属于TPYBoard，转载时请以超链接形式标明文章原始出处和作者信息及本声明

什么是TPYBoard v702
---------------------------

TPYBoard v702是山东萝卜电子科技有限公司最新开发的，目前市面上唯一支持通信通信功能的MicroPython开发板：支持Python3.0及以上版本直接运行。支持GPS+北斗双模通信、GPRS通信、短信功能、电话功能；板载温湿度、光敏、三轴加速度传感器、蜂鸣器、LCD5110显示屏。免费提供通信测试服务平台。

**TPYBoard v702实物图**

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170426/1493188183717935.png

TPYBoard v702实现来电显示
-------------------------------------------------------------------------------

**具体要求**

利用TPYBoard v702完成HTTP应用功能的测试，分别进行GET请求和POST请求。

**所需器件**

- TPYBoard v702开发板 1块
- LCD5110显示屏 1块
- SIM卡 1张(支持移动、联通)


板载通信功能及使用介绍
------------------------------

TPYBoard v702的开发板的整体整体亮点置一就是能板载通信功能，只要在开发板的卡槽上插上一张可以使用的手机卡（不支持电信），即可使用该功能。开发板板载的通信功能包括了电话，短信，GPRS等功能，在这个实验里面我们只使用电话这个功能。
开发板板载的通信功能已经设计的很完善，在接到来电的时候，会主动的把来电的信息通过串口4发送进来，这样一来作为用户的我们就是需要把数据进行相应的处理加显示就可以了。

实现过程
---------------------

为了方便测试，这里用PHP写了一个接收GET和POST请求的程序，代码非常简单，可以直接放到自己服务器上测试，也可以直接用，访问地址在下面源码中有。

**PHP测试程序**

.. code-block:: php

    <?php

    $backstr = "error";
    $getStr = $_GET["t"];
    if($getStr!=""){
        $backstr = "{\"status\":\"0\",\"method\":\"GET\",\"data\":\"".$getStr."\"}";
    }else{
        $postStr = file_get_contents("php://input");
        if($postStr!=""){
            $backstr = "{\"status\":\"0\",\"method\":\"POST\",\"data\":\"".$postStr."\"}";
        }
    }
    echo $backstr;
    ?>

**编写v702上的程序**

HTTP应用是通过板载的GU620模块来实现的，所以我们需要知道该模块HTTP应用的相关AT指令，才能去编写程序实现功能。
`点击下载AT指令说明文档 <http://old.tpyboard.com/downloads/docs/TPYBoard%20v702_GU620%E8%B5%84%E6%96%99.rar>`_

**GET请求实现代码**

.. code-block:: python

    import pyb
    from machine import SPI,Pin
    from pyb import UART
    import json

    #GU620模块初始化
    N1 = Pin('Y6', Pin.OUT_PP)#定义通信系统启动引脚
    N1.low()
    pyb.delay(2000)
    N1.high()
    pyb.delay(10000)#拉高拉低引脚，启动通信系统
    u2 = UART(4,115200,timeout = 50)#定义串口4，设置 波特率为115200
    #初始化 HTTP 应用
    u2.write('AT+HTTPINIT\r\n')
    getURLCMD = 'AT+HTTPPARA=1,"http://old.tpyboard.com/v702/httptest.php?t=123456"\r\n'
    #getURLCMD = 'AT+HTTPPARA=1,"https://www.baidu.com"\r\n'
    while True:
        if u2.any() > 0:
            dataRead = u2.read()
            print('_dataRead:',dataRead)
            print('-'*30)
            if dataRead.find(b'OK') > -1:
                #AT命令执行成功
                #判断是执行的哪一步操作
                if dataRead.find(b'AT+HTTPINIT') > -1:
                    #初始化HTTP成功
                    #设置 HTTP 参数值 设置url
                    u2.write(getURLCMD)
                elif dataRead.find(b'AT+HTTPPARA=1') > -1:
                    #HTTP参数设置成功
                    #发起GET请求获取数据
                    u2.write('AT+HTTPACTION=0\r\n')
                elif dataRead.find(b'AT+HTTPREAD\r\n\r\n+HTTPREAD') > -1:
                    #返回可用的数据信息，进行解析 获取到数据长度
                    datalen = dataRead.decode().split(':')[1].split(',')[0]
                    print('datalen:',datalen)
                    #从第0位开始 读取指定长度的数据
                    u2.write('AT+HTTPREAD=0,{}\r\n'.format(datalen))
                elif dataRead.find(b'HTTP/1.1 200 OK') > -1:
                    #成功读取数据后 停止HTTP应用
                    u2.write('AT+HTTPTERM')
                    #进行数据解析 提取出我们需要的信息
                    b = dataRead.decode()
                    c = "{"+b.split('{')[-1].split('}')[0]+"}"
                    #转成json对象 查看是否请求成功
                    jsonobj = json.loads(c)
                    print(jsonobj["status"])
                    pyb.delay(100)
                    break
            elif dataRead.find(b'ERROR') > -1:
                #AT命令执行失败
                if dataRead.find(b'AT+HTTPINIT') > -1:
                    #初始化HTTP失败 有可能是之前的没有停止等原因
                    #发送停止HTTP命令 再重新初始化
                    u2.write('AT+HTTPTERM\r\n')
                    pyb.delay(300)
                    u2.write('AT+HTTPINIT\r\n')
            else:
                if dataRead.find(b'\r\n+HTTPACTION: 0, 200') > -1:
                    #收到的数据提示 说明请求成功
                    #查询当前可用数据
                    u2.write('AT+HTTPREAD\r\n')


**运行效果**

.. image:: /img/1.png
                    
**POST请求实现代码**

.. code-block:: python

    import pyb
    from machine import SPI,Pin
    from pyb import UART
    import json

    #GU620模块初始化
    N1 = Pin('Y6', Pin.OUT_PP)#定义通信系统启动引脚
    N1.low()
    pyb.delay(2000)
    N1.high()
    pyb.delay(10000)#拉高拉低引脚，启动通信系统
    u2 = UART(4,115200,timeout = 50)#定义串口4，设置 波特率为115200
    #初始化 HTTP 应用
    u2.write('AT+HTTPINIT\r\n')
    getURLCMD = 'AT+HTTPPARA=1,"http://old.tpyboard.com/v702/httptest.php"\r\n'
    sendData = "I am TPYBoard v702!"
    while True:
        if u2.any() > 0:
            dataRead = u2.read()
            print('_dataRead:',dataRead)
            print('-'*30)
            if dataRead.find(b'OK') > -1:
                #AT命令执行成功
                #判断是执行的哪一步操作
                if dataRead.find(b'AT+HTTPINIT') > -1:
                    #初始化HTTP成功
                    #设置 HTTP 参数值 设置url
                    u2.write(getURLCMD)
                elif dataRead.find(sendData.encode()+b'\r\nOK\r\n') > -1:
                    #POST需要发送的数据设置成功
                    #发起POST请求
                    u2.write('AT+HTTPACTION=1\r\n')
                elif dataRead.find(b'AT+HTTPPARA=1') > -1:
                    #HTTP参数设置成功
                    #输入HTTP数据AT+HTTPDATA=<size>,<time>
                    #<size>:发送的数据字节数
                    #<time>:最大输入数据的时间以毫秒为单位
                    u2.write('AT+HTTPDATA={},100\r\n'.format(len(sendData)))
                elif dataRead.find(b'AT+HTTPREAD\r\n\r\n+HTTPREAD') > -1:
                    #返回可用的数据信息，进行解析 获取到数据长度
                    datalen = dataRead.decode().split(':')[1].split(',')[0]
                    print('datalen:',datalen)
                    #从第0位开始 读取指定长度的数据
                    u2.write('AT+HTTPREAD=0,{}\r\n'.format(datalen))
                elif dataRead.find(b'HTTP/1.1 200 OK') > -1:
                    #成功读取数据后 停止HTTP应用
                    u2.write('AT+HTTPTERM')
                    #进行数据解析 提取出我们需要的信息
                    b = dataRead.decode()
                    c = "{"+b.split('{')[-1].split('}')[0]+"}"
                    #转成json对象 查看是否请求成功
                    jsonobj = json.loads(c)
                    print(jsonobj["status"])
                    pyb.delay(100)
                    break
            elif dataRead.find(b'ERROR') > -1:
                #AT命令执行失败
                if dataRead.find(b'AT+HTTPINIT') > -1:
                    #初始化HTTP失败 有可能是之前的没有停止等原因
                    #发送停止HTTP命令 再重新初始化
                    u2.write('AT+HTTPTERM\r\n')
                    pyb.delay(300)
                    u2.write('AT+HTTPINIT\r\n')
            else:
                if dataRead.find(b'\r\n+HTTPACTION: 1, 200') > -1:
                    #收到的数据提示 说明POST请求成功
                    #查询当前可用数据
                    u2.write('AT+HTTPREAD\r\n')
                elif dataRead.find(b'AT+HTTPDATA') > -1 and dataRead.find(b'>') > -1:
                    #提示>符号 串口直接发送POST数据
                    u2.write(sendData)

                    
.. image:: /img/2.png

   
- `下载源码 <https://github.com/TPYBoard/TPYBoard-v702>`_
