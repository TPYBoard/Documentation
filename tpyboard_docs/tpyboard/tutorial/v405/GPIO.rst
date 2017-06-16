TPBasic_STM32F405RGT6的GPIO使用
==================================

一、什么是TPBasic_STM32F405RGT6最小系统板
--------------------------------

	TPBasic_STM32F405RGT6最小系统板是基于STM32F405RGT6芯片开发制作的最小系统板，方便开发与集成使用。
	

二、利用TPBasic_STM32F405RGT6最小系统板完成按键控制LED亮或灭
------------------------------------------------------------

1、具体要求
----------------------

	利用TPBasic_STM32F405RGT6最小系统板完成上电后灯亮一段时间然后灭，用按键按下控制LED灯亮，放开LED灯灭。

2、所需器件
------------------------

	- TPBasic_STM32F405RGT6最小系统板		一块

	- 杜邦线					若干根


三、制作主要过程
-------------------------

1、制作流程
----------------------

	将按键用杜邦线接在PC4与地之间，是按下按键IO口变为低电平
	将LED的正极接在3.3V电源处，负极接在PC11上，当IO口变为低电平时LED灯亮起，当IO口为高电平时LED灯灭。

2、主要代码
----------------------

.. code-block:: c

	#include "stm32f4xx.h"

	#define ON  0
	#define OFF 1

	#define key (GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_4))


	void LED_Configuration(void);
	void delay(unsigned long int time);


	void LED(a)
	{
		if(a)
		GPIO_SetBits(GPIOC,GPIO_Pin_11);
		else
		GPIO_ResetBits(GPIOC,GPIO_Pin_11);
	}

	/*
	 * 函数名：main
	 * 描述  ：主函数体
	 * 输入  ：无
	 * 输出  ：无
	 * 调用  ：外部调用
	 */
	int main(void)
	{

	    SystemInit();
	    LED_Configuration();
	    LED(ON);
			delay(6000000);
	    LED(OFF);
	 //   delay(6000000);
			while(1)
			{
				if(!key)
					{
						 LED(ON);
					}
				else
					{	
						 LED(OFF);
					}


	    }
	}


	/*
	 * 函数名：LED_Configuration
	 * 描述  ：LED GPIO口配置
	 * 输入  ：无
	 * 输出  ：无
	 * 调用  ：外部调用
	 */
	void LED_Configuration(void)
	{
	    GPIO_InitTypeDef  GPIO_InitStructure;  //声明一个 初始化GPIO口时候用的 结构体
	    RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOC , ENABLE); //使能PI口的时钟，否则端口是不工作的
	    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_11 ; //对结构体的GPIO_Pin对象赋值，声明要操作的是11端口
	    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_OUT;//对结构体的GPIO_Mode对象赋值，声明IO口的模式是输出
	    GPIO_InitStructure.GPIO_OType = GPIO_OType_PP;//对结构体的GPIO_OType对象赋值，声明IO口的结构是推挽输出
	    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_100MHz;//对结构体的GPIO_Speed对象赋值，声明速度是100MHz
	    GPIO_InitStructure.GPIO_PuPd = GPIO_PuPd_NOPULL; //对结构体的GPIO_PuPd对象赋值，声明不上拉
	    GPIO_Init(GPIOC, &GPIO_InitStructure);//将结构体带入初始化函数中，执行初始化，否则之前设置都是无效的

		  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4 ; //对结构体的GPIO_Pin对象赋值，声明要操作的是4端口
	    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN;//对结构体的GPIO_Mode对象赋值，声明IO口的模式是输入
	    //GPIO_InitStructure.GPIO_OType = GPIO_OType_PP;//
	    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_100MHz;//对结构体的GPIO_Speed对象赋值，声明速度是100MHz
	    GPIO_InitStructure.GPIO_PuPd = GPIO_PuPd_UP; //对结构体的GPIO_PuPd对象赋值，声明内部上拉
	    GPIO_Init(GPIOC, &GPIO_InitStructure);//将结构体带入初始化函数中，执行初始化，否则之前设置都是无效的



	}


	void delay(unsigned long int time)
	{
	    while(time--);
	}


