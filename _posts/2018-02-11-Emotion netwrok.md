---
layout: post
title:  Emotion Network
date:   2018-02-11 16:10:20
categories: Emotion
---
# Paper1 Kinnison et al., 2017

## Introduction

* 本文直接从认知神经科学的角度提出问题：过去几年脑网络的研究风生水起，但基本都集中在静息态研究中。目前任务态下脑网络属性的研究很少，这限制了人们对于脑网络的理解。
* 例如，dmPFC，前脑岛（anterior insula，AI），BNST和thalamus参与威胁信息的加工；而奖赏信息，除了这些脑区，还涉及腹侧和背侧纹状体。然而目前对于威胁和奖赏信息如何影响这些脑区的交互作用还鲜有人知。
* 当前的研究使用动机或情绪的线索（cues）来构建迷你状态（mini-states），研究这一阶段中脑网络属性的变化。

## Method
### Manipulation
* 情绪任务中，线索为威胁或安全信息；奖赏任务中，线索为奖赏或控制信息。目标刺激呈现时都让被试进行一个反应-冲突任务（response-conflict task）。
  ![Experimental paradigms](http://p24kfvgv3.bkt.clouddn.com/18-2-11/89516320.jpg)
* 在反应冲突任务中，要求被试判断图片的内容是房子还是建筑物（house or building），并且忽略图片上的字体。

### Analysis
* 分析1：个体分析，组分析和ROI分析。激活分析关注的就是从线索呈现后5-7s的时间段。进行组分析的主要目的是得到感兴趣对比的激活点；然后以激活的peak点坐标为中心点，制作半径为5mm的ROI，做为后续脑网络分析的节点。
* 分析2(关键)：Trial by trial responses分析（Rissman et al., 2004）。跟正常的个体分析一样，每个被试构建一个GLM设计矩阵，但是每个正确试次的线索刺激模拟为一个独立的事件。目标刺激则用一般的方法将一个条件模拟为一个事件。相比一般的GLM个体分析，这样做的优点在于可以得到不同脑区在实验任务不同时间阶段的功能连接，而前者只能得到不同脑区某个条件的功能连接，损失了时间信息。分析2的目的在于利用trial by trial functional connectivity分析得到脑网络的边，这是本研究的核心所在。
* 操作性检验：相关分析。由于线索和目标刺激之间的间隔没有都超过4s（情绪任务1.75-5.75，动机2-6s），所以作者在对情绪和动机两个数据集的分析1和分析2中，都对线索和目标刺激的协变量进行了相关分析。分析1两个数据集的相关结果表明相关都比较弱（情绪0.34，动机0.37），说明线索和目标刺激可以独立进行估计分析。分析2中的第二次相关分析则发现任何一个单独试次的协变量和目标刺激的协变量之间的相关程度较低（<0.2），表明单个刺激的分析是有意义的。
* 脑网络构建（network construction）。如分析1和分析2所述，节点和边的操作性定义已经确定。边的定义注意两件事，第一这里没有使用一般的皮尔逊相关，而是用了robust回归的方法，这样可以避免极值的影响；第二，本文将负相关，或者说负边的权重设置为了零。这是因为许多网络的测量手段不能处理负值权重，一个常见的方法是将正和负的网络分开处理，或者直接忽略负边。在本研究中，负边的意义不明确，而且负边的总量较少，组分析中没有发现负边，所以采用的第二种办法。
* 脑网络分析（network analysis）。这部分主要介绍一些脑网络的分析指标，也就是因变量。
* 统计检验（statistical tests）。对这些因变量指标进行什么样的统计分析。因为不知道moudularity，global efficiency，mean edge weight, within community和between community这些指标是否符合正态分布，所以为了进行显著性检验，这些值通过Wilconxon's signed-rank test（威尔科克森符号秩检验）来相互比较。作者还对脑网络内不同的边（只有不同群落的之间的边，不是所有ROI2ROI的边都采用了）的连接强度进行了统计检验。情绪检验有60个边，也就是60次检验；动机数据里有117个边，对应117次检验。因为统计次数很多，作者使用了FDR的方法进行多重检验矫正。

## Results
* 

**References**
* Kinnison, J., Padmala, S., Choi, J.-M., & Pessoa, L. (2012). Network analysis reveals increased integration during emotional and motivational processing. The Journal of Neuroscience, 32(24), 8361–8372. https://doi.org/10.1523/JNEUROSCI.0821-12.2012
* Rissman, J., Gazzaley, A., & D'esposito, M. (2004). Measuring functional connectivity during distinct stages of a cognitive task. Neuroimage, 23(2), 752-763.
