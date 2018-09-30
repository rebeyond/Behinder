# Behinder
“冰蝎”动态二进制加密网站管理客户端

原文链接：https://xz.aliyun.com/t/2799

##概述
本系列文章重写了java、.net、php三个版本的一句话木马，可以解析并执行客户端传递过来的加密二进制流，并实现了相应的客户端工具。从而一劳永逸的绕过WAF或者其他网络防火墙的检测。
本来是想把这三个版本写在一篇文章里，过程中发现篇幅太大，所以分成了四篇，分别是：

利用动态二进制加密实现新型一句话木马之Java篇
利用动态二进制加密实现新型一句话木马之.NET篇
利用动态二进制加密实现新型一句话木马之PHP篇
利用动态二进制加密实现新型一句话木马之客户端篇
##正文
在前面三篇文章中，分别实现了Java、.net、PHP三个版本的新型一句话木马，也针对不同版本编程语言的特点，对客户端的差异化部分做了简单的介绍。下面就把客户端的功能再简单介绍一下，就当是README.MD了，客户端暂时取名为冰蝎，采用Java+SWT开发，支持Windows、Linux、MacOS（Mac系统需通过-XstartOnFirstThread参数执行），主要功能如下：

###1.	基本信息
客户端和服务端握手之后，会获取服务器的基本信息，Java、.NET版本包括环境变量、系统属性等，PHP版本会显示phpinfo的内容。
###2.	文件管理
这个没什么好说的，无非是文件的增删改查，稍微不同的是上传的文件都是加密传输的，可以避免被拦截。
###3.	命令执行
执行单条操作系统命令。
###4.	虚拟终端
虚拟终端是模拟了一个真实的交互式Shell环境，相当于把服务器侧的Shell给搬到了客户端，在这个Shell里可以执行各种需要交互式的命令，如ssh、mysql。比如说：我们可以在这个Shell里去ssh连接服务器侧内网的其他主机，可以参考下面这个动图：
![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162425-3f32bef2-bfd3-1.gif)

当然，如果你习惯powershell，也可以弹个powershell出来，如下图：

![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162509-5989eb36-bfd3-1.gif)
###5.	Socks代理
虚拟终端功能其实就已经部分实现了内网穿透的能力，在Shell环境里做的所有事情都是在内网环境中的。不过为了方便使用其他工具，客户端还提供了基于一句话木马的Socks代理功能，一键开启，简单高效，可以参考如下动图：

![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162620-839c5d50-bfd3-1.gif)

顺便说一下，代理过程中所有的流量都是在socks的基础上封装了一层AES。  

###6.反弹Shell

反弹Shell是突破防火墙的利器，也几乎是后渗透过程的必备步骤。提到后渗透，当然少不了metasploit，提到metasploit，当然少不了meterpreter，所以冰蝎客户端提供了两种反弹Shell的方式，常规Shell和Meterpreter，实现和metasploit的一键无缝对接。请参考如下动图：

![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162737-b16662f8-bfd3-1.gif)

上图演示的是Meterpreter，当然常规的Shell也可以对接metasploit，就不演示了。

###7.数据库管理

常规功能，实现了数据库的可视化管理，放张截图吧：
![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162809-c4b25d8a-bfd3-1.png)

和常规管理工具不同的是，在Java和.NET环境中，当目标机器中没有对应数据库的驱动时，会自动上传并加载数据库驱动。比如目标程序用的是MySQL的数据，但是内网有另外一台Oracle，此时就会自动上传并加载Oracle对应的驱动。

###8.自定义代码

可以在服务端执行任意的Java、PHP、C#代码，这也是个常规功能，值得一提的是我们输入的代码都是加密传输的，所以不用为了躲避waf而用各种编码变形，效果请参考如下动图：

![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162856-e09cdbf6-bfd3-1.gif)

###9.备忘录

渗透的时候总有很多零碎的信息需要记录，所以针对每个Shell提供了一个备忘录的功能，目前只支持纯文本，粘贴进去自动保存：

![](https://xzfile.aliyuncs.com/media/upload/picture/20180924162934-f7650e12-bfd3-1.png)

目前就实现了这么多功能，由于是在业余时间完成，略有仓促，有不少地方需要完善，当然也可能会有不少隐藏的bug，所以打算暂时不开源了，等迭代两个版本优化一下再说。如果在使用中遇到什么bug，欢迎反馈给我。



最后，祝大家中秋节快乐！

本篇完。
