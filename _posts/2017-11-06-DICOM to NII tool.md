---
layout: post
title:  DICOM to NIfTI conversion
date:   2017-11-06 22:50:20
categories: toolbox
---


* title：The first step for neuroimaging data analysis:
DICOM to NIfTI conversion
* authors，来自著名的CRNL（Chris Rorden's Neuropsychology Lab）实验室 http://www.mccauslandcenter.sc.edu/crnl/。
* DICOM是大多数MRI机器输出的原始格式
* NIfTI是大多数数据处理软件输入的数据格式
* 虽然目前有很多软件可以在两者之间转换，但是他们往往只适用于一些特殊条件。某些dcm文件可以，另一些则可能报错。
* 本文的目的就是找出这些软件所使用的情况，以及它们会在什么情况下出现bug，如何解决。

* 工具包下载地址 http://www.mccauslandcenter.sc.edu/crnl/tools
