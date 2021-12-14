---
layout: post
title:  E-prime和PschoPy等刺激呈现软件的使用
date:   2018-04-24 22:50:20
categories: Programming
---

# Eprime教程

## 版本问题
  * Eprime版本复杂，不同版本之间存在严重的不兼容问题。低版本无法打开高版本，标准版无法打开专业版。
  * E-prime版本：学部不同实验室eprime2.0版本混乱，这台能跑的程序换台电脑不一定能正常运行。所以尽量用1.1.
  * Windows版本。windows7和一般的windows10版本没有问题。除了Windows 10 1703。目前1703是唯一一个运行E-Prime2.0 SP2（2.0.10.356）有问题的Win10版本，请一定避开。

## Eprime1.1使用的注意事项
 1. 不支持中文字符，中文要换成图片。
 2. Eprime1.1 图片只支持bmp格式，图片的分辨率最好为640*480，太大的bmp文件加载较慢，如果分辨率在1000*1000以上，很可能就会卡死。
 3. Eprime1.1 程序本身也需要进行分辨率设置！具体设置方法为双击“Experiment-Object”（图中所示1），在弹出的属性页面中选择“Devices”（图中所示2），选择“Display”，在弹出的窗口中进行相应的设置即可（图中所示3）。在没有很特别的要求下，1024*768，32位色即可。**注意，一般情况下调整分辨率无用的话，很可能就是色位的问题，改成32一般就可以解决问题**
 4. WIN10使用E-Prime需要DirectX 11 + 32位色+ display flipping，只有开启这些东西才能在WIN10上得到准确的显示时间；否则，得到的反应时数据可能是错误的。
![](http://p24kfvgv3.bkt.clouddn.com/18-5-18/63601256.jpg)

## Eprime 2使用注意事项

1. 因为学部核磁使用的是标准版，所以建议使用的版本为E-prime 2.0标准版-Build2.0.8.22。
2. Eprime该版本若要正常使用，必须安装Microsfot Net 3.5 framework。否则就会出现安装后打开程序报错，或者安装后点击图标没有反应的问题。windows XP或win7可以在官网下载安装包进行安装。Windowss 10如果点击安装包没有反应，则需要在“启用或关闭windows功能”中勾选Net3.5.
3. 安装完成后，建议删除2.0的安装文件夹下的Liscensemanager.exe程序，可以避免每次打开时挑出的激活对话框。

## Common Errors

1. [常见报错链接](https://support.pstnet.com/hc/en-us/articles/229359907-INFO-Common-E-Prime-Error-Numbers-Messages-and-Solutions-18011-)
2. Eprime的运行逻辑是先将GUI界面的程序转化为E-basic语句，真正运行的是语句。所以编程中一些需要注意的事项在这里同样适用。例如程序中每个对象的命名不可以与E-prime预设的变量名一样。例如“End”是E-prime程序本身预设的变量名称，如果把Fixation命名为End就会报错。

# E-basic 编程

## 编程基础

* E-objects and its attributes. E-Prime采用面向对象(“对象”，object，即为我们实验的组成部分)的设计方式，并通过对对象属性的设置达到对实验的控制。
* E-studio中有多种功能的控件，能够实现对不同实验对象的控制。例如TextDisplay控件可以对文本对象进行控制。
* 对象属性的表示方式为“name.attribute”，name为控件的名称，attribute为属性的名称。例如TextDisplay4.duration表示TextDisplay4这一对象的duration属性。
* 注释为“'”符号。

### 创建属性SetAttrib
* 创建属性并赋值。除了控件默认的属性，还可以利用**SetAttrib**创建自定义的属性并赋值。具体而言SetAttrib可以将创建的属性置于实验环境（context）中，因此其可以被指定，修改和引用。“c”默认为环境对象（Context）的缩写，不需要单独定义。例如：
    ``' Set TotalTrial =10
    c.SetAttrib "TotalTrial", "10"``
    将TotalTrial设置为contex对象的一个属性，并赋值为10. 注意set函数虽然也能给变量赋值，但其在其他控件中无法被引用。
* 属性的创建必须在其被引用之前，否则就会出现“the attribute does not exist”的runtime error。
* SetAttrib的引用问题。使用SetAttrib创建的变量只能在创建的环境中被引用，例如TotalTrial是在block水平上被创建的，如block1，那么其就只能在block1中被引用。
* context属性分为几个层级，session level context > Block level context > Trail level context.

### Global Variables and Trial Level Variables

* 全局变量需要在Script里进行设置，这样设置后可以在任一pro调用该变量。 Global variables are declared in the User tab of the Script window. The syntax is “Dim” followed by the variable name, followed by “As [Datatype]”, where datatype can be a string, integer, or other type.
* 局部变量在Inline控件中进行设置，只能在本语句中进行调用

# E-prime和fMRI的同步

1. 将指导语（开始界面）的RTTime勾选。指导语的RTTime表示的是e-prime系统接收到磁共振系统发送的第一个触发键（如s）的时间，
是e-prime行为系统和磁共振系统同步的起点。[参考1](https://www.andysbrainblog.com/andysbrainblog/2017/6/23/e-prime-tutorial-11-making-your-experiment-scanner-compatible)，
[参考2](https://www.chbh.bham.ac.uk/wiki/index.php/Code_Examples)
2. 注意，指导语的onset-time并不是同步时间。一般在e-prime运行之后，指导语界面已经打开。onset表示的是指导语界面出现的
时间。但是这时候磁共振系统还没有开始扫描，主试往往需要先保证被试能够看见并理解指导语之后，才会开始
扫描。

# Eprime常用语句

## 练习阶段语句

### 练习正确率达标再继续

正确率超过80％就中止练习
``第一部分：先在script处定义AccMean，语句：
public  AccMean as integer

 '第二部分：'inline 1——此inline放置在List所调用的Procedure末
if ***.ACC = 0 then
   AccMean = AccMean + 1
end if

 第三部分：'inline 2——此inline放置在List之后，用来判断
If AccMean>.80 then
  Pracblocklist.terminate
else
 AccMean = 0
 goto label1       '再次练习，跳到label1（放置在 Pracblocklist前）
End if  ``

### 练习反应时和正确率都达标后再继续

* 在练习阶段，被试完成任务的成绩同时满足以下两个条件：1、正确反应率在85%以上；2、正确反应的平均反应时在800ms以内，才可以进入正式实验，否则继续练习。
* 通过添加inline可以实现该功能，具体操作如下：
    1. 在User Script中声明全局变量。
``Public N
Public CorrectN
Public TotalRt
Public MeanRt
Public CorrectPercent``
    2. 在练习模块前后以及模块中添加控件和Inline。具体而言，在主时间轴上加两个控件，一个是在开头加上Label，一个是在练习部分结束后加上InLine。
    3. Inline的内容。
    Inline1：给变量赋值，该语句放在实验流程最开始的地方。
    ``N=0
    CorrectN=0
    TotalRt=0
    CorrectPercent=0
    MeanRt=0``
    4. Inline2累计正确反应数和反应时，放在trial最后
    if stimili.acc=1 then   
    CorrectN=CorrectN+1  
    N=N+1  
    else  
    N=N+1
    end if
    TotalRt=TotalRt + Stimili.Rt
    5. Inline3：计算平均反应时和正确反应率，并进行判断. 达标跳到label2（练习后，正式实验前），不达标跳到label1（练习之前）.
    ``MeanRt=TotalRt/N
    CorrectPercent=CorrectN/N*100
    if CorrectPercent>85 and MeanRt<800 then  
    goto label2
    else goto label1
    end if``

## 设随机间隔

1. 2-6S随机：   
    `` set TextDisplay4.duration= random (2000,6000)``
    解释： TextDisplay4为控件名称，(2000,6000)为随机刺激间隔。该语句相对于控件property中duration的设置会优先执行。   
    **这个必须写在相应控件的前面。**
2. 2000ms，4000ms和6000ms随机间隔
    ``TextDisplay1.duration=2000*random(1,3)``


## 选择语句使用

1. 假如反应为“1”或“3”，则标记为“1”或“3”；不反应则标记为“2”
    `` if Answer.resp=”1” or Answer.resp=”3” then
    WritePort&H378,c.GetAttrib(“Answer.resp”)
    else WritePort &H378,2
    endif ``
    “Answer”是刺激的名称，此句应放在“Answer”后面
2. 当反应的按键是字母（如j、f）时：
    “按f键读为１，按j键读为２，无需按键时读为３。”标在后面
    ``if Target.resp = "f" then
    writePort&H378,1
    elseifTarget.resp = "j" then
    writePort&H378,2
    elsewritePort &H378,3
    endif``
3. if语句还可以写成ifthen
    ``If Target.GetAttrib(“code”)=“5” and back.GetAttrib(“code2”)>0  and resp = "j" and Target. c.GetAttrib(“code”)=“5”
    then writePort &H378,3 ``
    如果Target获得的刺激类型5，同时反馈刺激为J 那么向端口输出，mark为3。 当然还可以继续增加And多个条件。
4. 被试休息语句
    `` dim a as Integer
    a=c.getattrib("List1.sample")
    if a mod 9=0 then
    msgbox("Itis time to rest,you havefinished"&int(a/9)&"/10"&",Press SPACE tocontinute")
    Endif ``
    说明: 每9个trial休息一次,休息时已经完成了全部trial的10分之(a/9),"10"可根据具体实验修改
5. 练习阶段后，让被试选择返回练习还是开始正式实验
    在练习阶段之后，添加Inline，写入以下代码：（注意：label1放置在练习阶段前；label2放置在练习阶段之后，正式实验前。）。
	【放置好label1, label2后，可直接将下列代码复制粘贴到Inlline中】
    ``Dim Selection(1) as String
    Selection(0)="继续实验"
    Selection(1)="重新练习"
    Dim DD as String
    DD=SelectBox("是否返回练习？","请选择",Selection())
    select case DD
    case 1
       goto label1      ‘label1放置在练习阶段前
    case else        ‘用“case else”而不用“case 0”，是因为这样可以包含被试点“cancel”或关闭窗口的情况

       goto label2     ‘label2放置在正式实验前。
    end select``

## 行为数据提取

* write the response of the stimulus to thedat file inorder to merge data

``dim x,y,z,a,g,j as string
    x=list1.order.count
    y=textdisplay1.resp
    a=c.getattrib("code")
    g=c.getattrib("stim")
    j=c.getattrib("resp")
    if textdisplay1.resp = "a" then y = 1 end if
    if textdisplay1.resp = "b" then y = 2 end if
    dim y1,z1,a1,x2,x3,x4 as integer
    x1=x1+1
   y1=val(y)
z1=val(z)
a1=val(a)
x2=x1+1
x3=val(g)
x4=val(j)
if textdisplay1.resp<>""then
write#1,x1,y1,a1,textdisplay1.acc,textdisplay1.rt,g
write #1,x2,0,y1,0,0,j
x1=x1+1
else
write #1,x1,y1,a1,textdisplay1.acc,textdisplay1.rt,g
x1=x1
end if
'write #1,x1,0,z1,1,0``


## E-prime和ERP-EEG的同步

 基本分为两步，第一步，打开端口；第二步，标记实验条件对应的mark到数据中。

* 开端口，就是准备向其他设备如ERP、fMRI发送trigger，使之能够记录到EPrime中的mark。   
* 每个要打mark的控件都要写这样一组语句。

1. inline1，打mark前的准备:
    - 打开(onset)端口命令：
    ``SoundOut1.OnsetSignalEnabled= True '请打开控件SoundOut1的端口   
SoundOut1.OnsetSignalPort= &H378  '打开SoundOut1端口为H378（固定的）``
    - 关闭(offset)端口命令：
`` SoundOut1.OffsetSignalEnabled= True  关闭控件SoundOut1的端口   
SoundOut1.OffsetSignalPort= &H378  关闭控件SoundOut1的端口为H378 ``
2. inline2 打mark :
    向ERPs发送控件SoundOut1的刺激信号（Mark），这个刺激信号从List中的condition（code）获取:
    ``SoundOut1.OnsetSignalData= c.GetAttrib("Condition")``
3. inline3 打mark
    ``writePort &H378,0
   target1.OnsetSignalData = c.getattrib("tmark")``
    前一句是将系统记录归零，如果不归零，可能会打上一些莫名其妙的，让人头疼的幽灵mark!  引号中的tmark是要调用的list中的属性，即事先在list中定义该控件在不同条件下的mark，然后引用，这样还是比较方便的，适合一些有规律的mark。这个写在target1控件前面的inline中。mark必须用数字，并且只能用256个自然数，大于256的数字就不能识别了
4. inline4 打mark
     ``if target1.Acc = 1 then
    writePort &H378,7
    elseif target1.Acc = 0 then
    writePort &H378,8
    else writePort &H378,9
    end if``                                            
     也是打mark，这个是直接根据反应打mark的。.Acc也可以换成.RESP等在logging中收集信息的项目。writePort &H378,后面的数值就是mark。   
     注意：elseif是写在一起的。上例中7和8是并列的，9是在被试没有反应时打的mark。但是此时ACC一般记为0，所以可能打的mark是8而不是9.



# Psychopy

* Psychopy的使用逻辑和Eprime基本一模一样，没有本质区别
* 时间的设置：Eprime里的infinit，Psychopy对应留空
* 变量的设置：
    1. Eprime里的[]调用对应Psychopy的美元符合$
    2. Eprime利用list来实现循环，Psychopy利用Excel填入变量名称和对应值，对其引用来实现循环
* 编程：Pychopy的if语句带冒号，注意缩进


# 参考资料
* [Win10 1709/1803 安装及使用E-Prime 2.0 笔记（各种避坑）](https://zhuanlan.zhihu.com/p/34113852)
* [e-prime常见问题Q&A分享](https://tieba.baidu.com/p/4045706988?red_tag=1916506853&traceid=)
* E-prime 的Inline语句 http://blog.sina.com.cn/s/blog_b43ab1510102vvh3.html
* https://support.pstnet.com/hc/en-us/articles/115013098487-SCRIPTING-Examples-and-Exercises-22904-
* [导言与目录：关于轻松学习E-Prime](https://www.jianshu.com/p/5bbf1c274370)
* [E-Prime Tutorial #8: Inline Objects](https://andysbrainbook.readthedocs.io/en/latest/E-Prime/E-Prime_ShortCourse/EP_08_InlineObjects.html)
