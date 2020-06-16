---
layout: post
title:  aa toolbox
date:   2017-11-01 22:50:20
categories: toolbox
---

# aa 介绍
1. aa工具包使用模块化的思想来处理MRI数据处理。
2. 其组织逻辑可以分为两大块：做什么和谁来做。
3. [官网](http://automaticanalysis.org/)
4. [Github](https://github.com/rhodricusack/automaticanalysis/)
5. 基于matlab和xml语言，只能在Linux or MacOX 下运行！不支持Windows！

# 基本知识：xml语言

* XML 指可扩展标记语言（EXtensible Markup Language）
* XML 仅仅是纯文本，不会执行任何操作，只用于在模块之间传递信息。类似与HTML，区别在于XML被设计用来传输数据。HTML 被设计用来显示数据。
* XML 标签没有被预定义。需要自行定义标签。xml逻辑结构为树结构：http://www.w3school.com.cn/xml/xml_tree.asp。
* xml基本句子结构为三块：<开始标签> 内容 </结束标签>
       1. 每一句必须有开始（<字符串>表示）和结束标签，用</字符串>表示，字符串可以自定义，但开始和结束必须一致。例如<e>This is an example</e>。
       2. XML 的属性值须加引号：<note date="08/08/2008">这是个笔记 </note>
       3. XML标签和内容的字符串无需加引号。
       4. 注释：<!-- This is a comment -->

# 1.1 做什么（使用xml语言创建config文件）

## aa一共需要配置两个config文件

## aap_parameters_user.xml

1. 用以定义具体的参数（怎么干，parameters）。例如数据根目录
2. aap_parameters_defaults.xml示例，例如：
`` <rawdatadir desc='Directory to find BIDS data' ui='dir_list'>/data/fmri/aa</rawdatadir>
<specialseriesdirname desc='Subdirectory in subject dir for special series' ui='text'>func</specialseriesdirname>
<structdirname desc='Subdirectory in subject dir for T1' ui='dir'>anat</structdirname>``
      * T1dirname为标签，定义结构像所在文件夹的名称。
      * 每个标签有多个属性，desc用于描述（description简写）标签的作用，使用等号赋值。
      * ui用于定义标签的类别，常见的类别有文件夹（dir），文本（text），逻辑值（yesno），可选项（optionlist）
      * anat为标签structdirname的值，赋值符号为“>value<”，即结构像所在的文件夹命名为‘anat’。
3. 官网推荐参考aap_parameters_defaults.xml并结合本地电脑的配置环境，创建一个适合本机的aap_parameters_user.xml。
	  
### aap_tasklist_taskid.xml

1. 用以定义需要执行的数据处理步骤。例如预处理（slice timing，motion correction），个体分析（model estimation）和组分析
2. aap_tasklist_demo.xml文件是4级结构，
```
<aap> <!-- 第1级 -->
    <tasklist> <!-- 第2级 -->
        <initialisation> <!-- 第3级 -->
          <module><name>aamod_checkparameters</name></module>  <!-- 第4级初始化模块，默认即可 -->
        </initialisation>
        <main> <!-- 第4级自定义模块，只有这里需要修改！ -->
        <module><name>aamod_autoidentifyseries_timtrio</name></module>
        </main>
    </tasklist>
</aap>
```
前两级默认，只需要根据自己的需要添加或删除4级<main>中的内容。

## 加载config信息和运行

1. 使用aarecipe函数加载aap_parameters.xml和aap_tasklist.xml文件。
默认格式为： aap=aarecipe('[parameter_XML]','[tasklist_XML]');
例如：``aap = aarecipe('aap_parameters_defaults.xml','aap_tasklist_demo.xml');``

2. 全部的设定都存储在一个matlab结构数组里面，称之为‘aap’。 ‘aap’包括多个成分，用以存储不同的参数，也包括SPM的默认设定。
aa工具包的aap相当于spm中的batch。异名而同义，都是把所有的参数设置先存储在一个结构数组里面，最后再加以调用。

3. 使用引擎（engine）运行参数。实现函数：``aa_doprocessing(aap);`` 
类似于conn_batch(batch)和spm_batch(batch)函数。
https://www.youtube.com/watch?v=SMgFNTKM5CI&list=PLz67yl6N69XFFN2WIPAVY_kxXECBtJT6A&index=7

# 2. aa 操作

## 准备
1.	下载和安装，matlab，set path，“Add Folder”，
2.	Demo。位于aa工具包examples文件夹下，aa_user_demo_v2.m。
数据下载：http://automaticanalysis.org/getting-started/worked-example/

##  aa的使用逻辑
1. 首先需要将一些独立于具体实验研究的参数配置到parameters和tasklist文件中，例如spm文件夹（spmdir），而依赖于具体研究的参数（slices，TR，datadir）等留空。
**注意，在windows系统下，路径名中不需要包括盘符，例如spm安装路径为D:\Matlab_tools\spm12，只需要输入'\Matlab_tools\spm12'！其他依赖工具包类似**
2. 然后，在user scripts中，先运行aap=aarecipe('[parameter_XML]','[tasklist_XML]')加载配置号的参数文件；
3. 再然后，在user scripts中，对依赖于具体研究的参数进行定义；
