[Micropython][TPYBoard-ESP8266开发板]之固件的烧写
====================================================



今天，带大家一起玩一下最新的开发板TPYBoard-esp8266开发板，这是一款esp8266版的micropython开发板。

一、准备工作
----------------

1.TPYBoard-esp8266开发板	1个
2.USB数据线 1条
3.电脑 1台（嘻嘻，本篇文章以windows7系统为例）

二、烧写固件
-----------------

第一步：下载烧写固件所需的软件

	在windows系统下可以使用ESPFlashDownloadTool_v3.3.4软件。不用担心不好找，见下面的附件压缩包。
			 
	`flash_download_tools_v3.3.4_win.zip <http://www.tpyboard.com/ueditor/php/upload/file/20170222/1487749912530684.zip>`__ 

第二步：连接TPYBoard-esp8266开发板
	 通过USB数据线将电脑和TPYBoard-esp8266开发板连接起来，会自动安装USB转串的驱动。
	 安装完毕后，查看设备管理器，是否正确创建串口。我的电脑是COM44

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170222/1487749376138909.png

	 打开ESPFlashDownloadTool_v3.3.4软件，根据下图进行设置。
					
	`flash_download_tools_v3.3.4_win.zip <http://www.tpyboard.com/ueditor/php/upload/file/20170222/1487750849450572.rar>`__ 

	选择附件中的bin固件文件，地址0x00000，串口根据自己的实际情况选择，小编我的是COM44，波特率115200

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170222/1487750143795401.png

	设置完毕后，点击START。提示上电同步。

.. image:: http://micropython.net.cn/ueditor/php/upload/image/20170222/1487750496548782.png

	这时你需要让板子进入烧写模式。按住板子上的SW2(FLASH)按键不松，同时按一下SW1(RST)按键松开。
	界面显示下载中。。。  同时右边区域显示设备的MAC地址，此时只需要等待下载完成即可。

.. image:: http://www.tpyboard.com/ueditor/php/upload/image/20170222/1487750528571423.png
