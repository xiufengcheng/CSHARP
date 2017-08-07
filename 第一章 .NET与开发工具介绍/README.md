# **第一章  .NET与开发工具介绍**

## **.NET的由来**？
&emsp;&emsp;<font color = Brown>这是一个商业故事，事情还得从80年代说起...<br>
&emsp;&emsp;当时个人电脑PC兴起，微软（[Microsoft], [DOS系统]）与因特尔（[Intel], 芯片制造商）组成的Wintel联盟所向披靡，它们踩在蓝色巨人[IBM]（PC机,大型机制造商,Fortran）的肩上发达起来......关于更早计算机应用的故事，请看电影[《隐藏人物》]。整个80到90年代，微软Windows操作系统一度控制着整个PC市场，获得巨大成功
 <br><font color = black size =4> **Windows 1.0** (1983) </font> <br>![](./img/1.jpg) 
 <br><font color = black size =4> **Windows 2.0** (1987) </font> <br>![](./img/2.jpg) 
 <br><font color = black size =4> **Windows 3.0** (1992) </font> <br>![](./img/3.jpg) 
 <br><font color = black size =4> **Windows NT** (1993)  </font> <br>![](./img/5.jpg) 
<br>
&emsp;&emsp;微软的发展，被另外一家以生产浏览器软件而闻名的网景公司(Netscape)抢占了先机，它先于微软推出浏览器软件...<br>
&emsp;&emsp;当时，[SUN]公司以生产UNIX服务器为主，这种服务器可以用于网站建设，所以它始终坚持"网络就是电脑"
(The Network Is the Computer)的信条。当时，SUN公司也尝试设计一种名为Java的程序语言，一开始准备将其用在家用电器市场，因为Java代码灵活小巧，可移植性强，并能够跨平台操作(只需要装一个JAVA虚拟环境JVM，不同平台安装不同版本的JVM，Java只负责将.java翻译成.class,jvm将.class翻译成机器码，这样JAVA程序就能够被各种机器识别)，不局限于某种操作系统。
 <br><br><font color = black size =4> **Java**</font> <br>![](./img/java.jpg) <br>
&emsp;&emsp;1995年，互联网大行其道，SUN立刻意识到这是一个机会！因为Java适合用于编写浏览器软件。由于SUN当时对开发软件产品尚缺乏信心，所以它免费将Java放在网上，任由人们使用，没有想到此举使Java获得极大的成功和好评。<br>
&emsp;&emsp;微软为了打败Netscape，跟SUN达成某项交易，SUN允许微软使用Java言来开发IE浏览器，当IE成功问世时，微软将IE捆绑进操作系统中，使得很多电脑用户不得不使用IE作为缺省的浏览器软件。网景受到强烈的冲激，1996年春，网景拥有87%的市场占有率，1998年就降到40%，最后它也不得不免费供用户使用，终于在1998年11月，网景被美国在线(AOL)收购。<br><br>
&emsp;&emsp;**那么原本是亲密战友的SUN与微软是如何翻脸无情？以至Java最后也被Windows无情地抛弃呢？**<br><br>
&emsp;&emsp;**Java成了Windows中不受欢迎的客人** <br>
&emsp;&emsp;1996年9月的某个星期日，微软资深的软件工程师提笔给比尔·盖茨写了一封信，在信中,他们非常恳切地提醒比尔·盖茨注意一个正在形成的威胁，这个威胁不是来自别人，正是其盟友SUN公司创建的一种编程语言--Java，这种语言允许编程者一次性编写程序代码、就可以在多个不同的操作系统上运行(如从IBM的大型机到Sun公司的Unix服务器，再到WindowsPC机都能运行，甚至在手机平台上也可以)；而不需要针对每个计算机硬件和操作系统配置的不同而改动程序代码，并且这种语言在网络上是安全的。在信中，他写到："必须意识到Java不仅仅是一种语言，如果它仅只是一种语言，对微软是不会造成威胁的。我们愿意并且能够容易地为它建立最佳的表现形式，事情可以圆满解决了。但是事实上，Java绝不仅仅是一种语言，它是COM的替代者。"--而COM恰恰是Windows基于的编程模型。"
&emsp;&emsp;盖茨在收到这封信时，正是他准备"闭关清修"的前几天，原来比尔·盖茨每年都要抽出一点时间来考虑微软长期发展战略，人称"思考周计划"。盖茨显然被这封信吓坏了，他第二天就回信了："这可把我吓坏了。我不清楚微软的操作系统要为Java的客户应用程序代码提供什么样的东西，而这些东西将足够让它来取代我们的市场地位。了解这一点非常重要，是应该最优先考虑的事情。"(没想到，这封信成为几年后司法部针对微软的反托拉斯案的呈堂证供。)于是，Java成了Windows中不受欢迎的客人，微软开始对其进行清扫，SUN又岂是好惹的？一场针尖对麦芒的好戏就开场了...
#### Java成了Windows中不受欢迎的客人 <br>
</font>

### Microsoft. NET 战略
* 让时光回到1995年...Java的问世极大的刺激了对于Windows平台应用极度看中的微软
* 
* 2000年6月，微软推出了
* 


<!-- 下面是本文档中用到的链接 --->
[《隐藏人物》]: https://baike.baidu.com/item/%E9%9A%90%E8%97%8F%E4%BA%BA%E7%89%A9/2454257?fr=aladdin
[DOS系统]:http://www.pc811.com/xitong/26367.html
[Microsoft]:https://www.microsoft.com/
[Intel]:https://www.intel.com/
[IBM]:https://www.ibm.com/
[微软与Netscape之间的恩怨]: https://en.wikipedia.org/wiki/Netscape
[SUN]:https://www.oracle.com/sun/index.html