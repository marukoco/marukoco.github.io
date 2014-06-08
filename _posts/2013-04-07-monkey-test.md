---
layout:post
title:Monkey test
---
做Android手机测试，我们引进的自动化测试工具还是挺少的，大部分依靠手工测试（大家写case，盯着手机点点点~），还有部分RD自己开发的简单自动化测试Tool，比如：Power on/off sress test，Wi-Fi Download Stability Test，Wi-Fi ON/OFF Stability test，2G/3G on/off stress test等，这些虽然看起来功能很简单，但也确实为我们的工作减轻了很多负担。Android中比较常用来执行自动化测试的要数Google在Android API里提供的Monkey了，关于这个小猴子的介绍可以在Android开发网站的Developers/Tools Help下找到相关介绍和帮助。
[http://developer.android.com/intl/zh-CN/tools/help/monkeyrunner_concepts.html](http://http://developer.android.com/intl/zh-CN/tools/help/monkeyrunner_concepts.html)

我们的测试中，Monkey主要应用在压力和可靠性测试上，在搭建好的测试环境上，连接并设置好待测设备，然后运行测试脚本。之后就让猴子自己瞎点吧，所以手机放着一会唱歌，一会闹钟，各种发出奇怪的声音或显示奇怪的东西，不要惊讶，这都是猴子在搞怪。一般遇到手机ramdump、system crash、reboot等严重问题就会停下来，如果发现了就把log给抓下来，找RD来分析吧。如果没达到预期标准（能一次跑24h等），那就fail了，需要把相关问题解掉，然后出版本再跑，一般都是跑5~8只手机取平均值（资源限制啦，跑的越多值越精确）。

Monkey的Critical作为判定可否出货的一个标准，所以整个测试中都很重视，每个版本都要做这样长时间的测试，并做详细的测试报告。

知道了它在项目中的运用，下面就来看看Monkey的配置和参数：

![Alt text](\image\monkey\Configuration.png "Monkey configuration") 

Tag <monkey>
配置Monkey的参数
duration_min属性，指定monkey运行的总时长

Tag <package>
指定package，将以指定单个package的方式运行monkey
shell-extend属性，为AP设置和数据准备而单独指定script
device属性，为package运行monkey指定device

Tag <freerun>
配置freerun的参数
duration_min属性，为不同的free run plan提供运行时长设定





