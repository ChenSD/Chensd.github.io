---
layout: post
title:  Task fMRI and resting fMRI
date:   2018-04-12 16:10:20
categories: Brain
---

# Task fMRI to resting fMRI

## Cheng 2017
* 静息态功能连接（FC）的分析已经取得了巨大的进步。这一方法得以让我们检验与精神疾病相关的异常连接。静息态fmri方法相比任务态的优势在于，没有任何实验任务，所以可以避免组间在任务表现上潜在差异的混淆。
* 但是目前静息态数据分析存在两大挑战：第一，如何确定显著变化的FC；第二， 如何解释这些结果，搞清楚潜在的神经生物学系统，以及它们对疾病的影响。FC定义为两个脑区之间BOLD信号时间序列的Pearson相关。也可以视为一个边，脑区之间相关的程度就是边的强弱。
* 对FC生物学意义进行解释的重要途径就是使用任务态fmri找到这些脑区涉及的功能。有证据表明任务态fmri主要的功能网络与静息态fmri中所观测到的自发波动相匹配（Cole 2014； Smith 2009）。
* 脑网络在任务态和静息态之间的一致性让作者（Cheng 2017）找到了一种新的方法：利用Neurosynth的方法确定与不同任务和功能相关联的脑网络（Yarkoni 2011），然后再利用静息态fMRI确定哪一个网络在正常人和病人组之间存在差异。相对于之前一些研究只使用特定任务态数据来确定ROI（Koyama 2011；Burrows 2016），这种方法分析全脑的连接，并不局限于单一的FC，无疑提供了更加全面的研究视角。
* 本研究主要的目的就是开发一种确定静息态功能网络的新方法，然后利用这一方法检验病人组和控制组在静息态没有任务的时候，功能脑网络是否发生了改变。不过需要注意的是，这一分析是在全脑脑网络水平进行的，而非某些脑区之间某个连接。
* 本研究的主要步骤为：
  1. 先利用[Neurosynth](http://www.neurosynth.org/)来得到与任务相关的ROI。具体办法就是使用与特定任务相关的关键词在Neurosynth进行了219次搜索，每次搜索得到的坐标用来定义ROI（每个大约包括25个体素）。
  2. 然后在病人和正常组分别计算这些FC（or circuits）；
  3. 再然后,利用 **富集分析（Enrichment Analysis）** 来检验组间差异，即利用一个指标（Kolmogorov-Smirnov-like statistic, the enrichment score）来确定两组间哪一个FC有差异。
* 为了实施这一想法，研究者开发了一种名为Konwledge based functional connectivity Enrichment Anlysis (KEA)的分析方法来探讨FC水平上的组间差异。这一方法基于Gene Set Enrichment Analysis (GSEA)，但进行了改变以提高统计检验力。

## 富集分析
* 首先要了解基因的“过表达”。本来基因表达是受到各种内外信号的精确调控的，一旦这种调控机制的任何一个环节出现问题就会失控。基因的表达过程就可能进入失控状态，就有可能表现为表达过度，即该基因被过度的转录、翻译，最终基因表达产物超过正常水平。
* 基因富集分析(gene set enrichment analysis)是在一组基因或蛋白中找到一类过表达的基因或蛋白。


## Tusche 2014

* 情绪往往由外在的事件而诱发。但是在没有外在事件刺激的情况下，内在的清醒思考（waking thoughts）也能诱发强烈的情绪体验（如愤怒和喜悦）。
* 研究问题的提出：as subject driven, self-generated mental states are a major element of our lives, it is surprising that we yet lack a rigorous understanding of how they occur and are processed in the brain.
* 实验设计。总体思路是先让被试进行一个正负性情绪诱发任务，然后间隔一周，再让被试进行一个静息态扫描。在这个静息态扫描过程中，被试间隔30S需要对自己的自发性想法进行一下评分（thought samples），包括效价，过去未来和自我-他人三个维度。被试在静息态所有的效价评分进行平均，可以得到一个静息态效价偏向的指标（average valence of task-free thoughts during rest），表示被试在静息态过程中是偏向正性还是负性加工。
* 数据分析。先使用MVPA对任务态数据进行分析，得出mOFC是对情绪刺激进行思考的关键脑区。然后：
  1. 用mOFC在任务态的激活做ROI，然后拿到静息态中再进行MVPA分析。Multi-voxel response patterns in the mOFC-ROI reliably predicted the valence of self-generated mental states during task-free rest prior to thought samples (Session
B).
  2. 再用mOFC的模式指标（degree centrality）预测静息态的平均效价。



## References

* Cole, M. W., et al. (2014). "Intrinsic and task-evoked network architectures of the human brain." Neuron 83(1): 238-251.
* Smith, S. M., et al. (2009). "Correspondence of the brain's functional architecture during activation and rest." Proceedings of the National Academy of Sciences of the United States of America 106(31): 13040-13045.
* Cheng, W., et al. (2017). "Functional connectivity decreases in autism in emotion, self, and face circuits identified by Knowledge-based Enrichment Analysis." NeuroImage 148: 169-178.
* Tusche, A., et al. (2014). "Classifying the wandering mind: revealing the affective content of thoughts during task-free rest periods." NeuroImage 97: 107-116.


# Resting fMRI to Task fMRI or behavioral performance


## References

* Mennes, M., et al. (2010). "Inter-individual differences in resting-state functional connectivity predict task-induced BOLD activity." NeuroImage 50(4): 1690-1701.
* Sakaki, M., et al. (2013). "Amygdala functional connectivity with medial prefrontal cortex at rest predicts the positivity effect in older adults' memory." Journal of Cognitive Neuroscience 25(8): 1206-1224.
* Tian, L., et al. (2012). "Regional homogeneity of resting state fMRI signals predicts Stop signal task performance." NeuroImage 60(1): 539-544.
* Zou, Q., et al. (2013). "Intrinsic resting‐state activity predicts working memory brain activation and behavioral performance." Human Brain Mapping 34(12): 3204-3215.

* Craddock, R. C., Holtzheimer, P. E., Hu, X. P., & Mayberg, H. S. (2009). Disease state prediction from resting state functional connectivity. Magnetic resonance in Medicine, 62(6), 1619-1628.

# Structural MRI to Resting fMRI

* Honey, C. J., Sporns, O., Cammoun, L., Gigandet, X., Thiran, J. P., Meuli, R., & Hagmann, P. (2009). Predicting human resting-state functional connectivity from structural connectivity. Proceedings of the National Academy of Sciences, 106(6), 2035-2040.
* Goñi, J., van den Heuvel, M. P., Avena-Koenigsberger, A., de Mendizabal, N. V., Betzel, R. F., Griffa, A., ... & Sporns, O. (2014). Resting-brain functional connectivity predicted by analytic measures of network communication. Proceedings of the National Academy of Sciences, 111(2), 833-838.


# 
