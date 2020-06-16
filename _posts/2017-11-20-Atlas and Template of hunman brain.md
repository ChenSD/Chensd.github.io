---
layout: post
title:   Atlases and Templates
date:   2017-11-20 9:10:20
categories: Brain
---

# Basic terms

* The terms atlas and template refer to different things.
* Template 模版，一般指标准化的坐标体系（coordinate frame ），用于对个体被试的大脑进行标准化操作（normalization）——把不同个体不同形态的大脑配准到同一个标准空间以便于相互比较。但是，template并非包括标准的坐标点系统，一般也包括与坐标点对应的结构名称（e.g. ICBM brain template and Talairach Atlas）。
* Atlas 图谱，指对映射在同一个坐标体系中的多个被试的大脑数据进行分割（结构/功能）之后，所得到的包括大脑不同脑区名称（lables）和空间信息（spatial location/xyz）的“大脑地图”；我们一般用图谱来定义自己感兴趣的脑区，也可以由坐标点找到对应的lable，反之亦可。
* 虽然有资料认为template和atlas是两个不同的术语（see https://web.stanford.edu/group/vista/cgi-bin/wiki/index.php?title=Atlases_and_Templates&oldid=1339），但是实际情况比较混乱，例如Talairach Atlas，即是一个标准化的坐标体系，也包括Brodmann area (BA) labels
* 但是，不考虑术语的标准化问题，大脑模板或图谱所包含的信息，从本质上可以分为两类，空间信息（coordinates）和分割信息（parcellation）。

# Talairach/MNI templates and spaces
## Talairach
* Talairach—Tournoux脑图谱的标本来源于一个无神经系统病变的56岁法国妇女尸体。1988年法国科学家Talairach和Toumoux将冰冻后的大脑标本每间隔2mm一5mm做矢状切片，对各剖面进行等距拍照。在所获得的照片中，手工分割出各个解剖结构和功能分区的轮廓线。用同一种颜色或图纹标记同一个结构或功能分区。通过分析矢状断面来绘制水平、冠状方向的脑结构功能图谱。该图谱对脑深部核团的解剖功能区进行了定义，包括尾状核、壳核、苍白球、红核、齿状核、腹后外侧核、腹后内侧核、腹后核等重要功能区。
* Talairach图谱定义了人脑的解剖坐标体系(Talairach Coordinate Space)，原点为以前连合后缘（Anterior Commissure，AC）。
* Talairach—Tournoux脑图谱是在假定脑对称的前提下完成的，因此在接近前联合的脑区较准确，而在靠近大脑新皮层和两大脑半球高度非对称脑区(如颞叶)的正确性显著下降 (李坤成，2010)。
* see http://www.talairach.org/about.html for more information.

## MNI   
* MNI means Montreal Neurosciences Institute (蒙特利尔神经研究所).
* 由于Talairach的样本只有一个个体，代表性较差，所以MNI想要定义一个更具有代表性的标准脑。
* MNI将大量正常个体结构像线性配准到Talairach空间，然后将配准后的脑图像平均得到了新的立体脑模板。
* 由于该模板是通过平均得到的，所以是一个基于统计概率性的脑模板（Evans et al., 1992）.
* 第一个MNI模板是MNI305 （Evans et al., 1993），其定义的脑空间即著名的MNI坐标体系。
* 目前使用更为广泛的是脑成像国际联盟（International Consortium for Brain Mapping，ICBM）公认的ICBM152模板，其实也由MNI出品。这个标准大脑模型来自152名年轻成人的高空间分辨率扫描结果。研究者将这些大脑通过仿射转换之后与MNI305进行对应，再将这些152个大脑进行平均，得到了更为清晰的标准模板。
* 这个模板之所以不叫做MNI152而是ICBM152，是因为它被ICBM采用作为标准模板 (see http://imaging.mrc-cbu.cam.ac.uk/imaging/MniTalairach).
* The MNI 152 is the averaging of 152 3DT1 coming from 152 different healthy peoples using 9 parameters. It means that when you visualize the MNI152 image, you see an "average brain".  
* 随后，脑成像国际联盟ICBM又推出一个更具有代表性的模板：ICBM452，将452个人大脑通过转换与ICBM305匹配之后的结果，但是目前ICBM452的使用范围比较小。
* 不管是MNI305还是ICBM152模板，其清晰度都差强人意（平均的原因？）。为了得到更加清晰的大脑图，蒙特利尔神经研究所对一位研究人柯林·福尔摩斯（Colin Holmes）的大脑进行了27次扫描，将这些扫描的结果与MNI305进行配准，然后平均起来得到了更加清晰和精确的大脑图，这就是柯林27（Colin27）标准大脑图。目前，许多基于MNI大脑模板的神经成像结果图均是在柯林27这个图像上进行显示 （Holmes et al., 1998）。
* 按照时间从现在到过去的顺序，ICBM452 -> ICBM152 -> MNI152 -> MNI305 -> Talairach -> Brodmann

## Differences between MNI brain and Talairach
* see http://www.nil.wustl.edu/labs/kevin/man/answers/mnispace.html for a disscussion

****
References
* Bias between MNI and Talairach coordinates analyzed using the ICBM-152 brain template.
* A probabilistic atlas and reference system for the human brain: International Consortium for Brain Mapping (ICBM)''. Philos Trans R Soc Lond B Biol Sci. 2001 Aug 29;356(1412):1293-322.

# 常见的大脑分割图谱
## 基于结构像或细胞形态学

### AAL 基于结构像分割
* The Colin27 brain, non-linearly warped to MNI152 space, has been segmented into 45 volumes per hemisphere by TzourioMazoyer et al. (2002) to generate the well-known Automatic Anatomical Labeling (AAL) atlas. This atlas also includes a cerebellar parcellation (Schmahmann et al., 1999, 2000)
* AAL全称是Anatomical Automatic Labeling，AAL分区是由MNI机构对Colin27标准脑进行分割得到的。AAL模板一共有116个区域，其中90个属于大脑（左右脑各45）结构，剩余26个属于小脑结构。
* Rolls等人在2015年对OFC进行了更细致的划分，创建了AAL2。
* 在线查询工具 http://qnl.bu.edu/obart/explore/AAL/

### Brodmann 基于细胞结构像
* 20世纪初，德国神经解剖学家Korbinian Brodmann、Alfred Walter Campbell、Grafton Elliot Smith、Cecile、Oskar；Vogt等人组成研究小组，对人类大脑皮质开始组织学研究，开创了人脑图谱研究新纪元。Brodmann及其研究小组把不同大脑区域做成组织切片，并进行Nissel和Heidenhain—Woelcke染色。通过观察发现：大部分脑皮质都由6层不同的神经元细胞构成，但不同部位切片中的神经元细胞在大小、形态、染色浓度上存在明显的区域差异，髓质的分布类型也不相同。按照细胞构筑的区域性差异，Brodmann将人脑皮质划分成52个不同区域；
* Brodmann分区是一个根据细胞结构将大脑皮层划分为一系列解剖区域的系统。神经解剖学中所谓细胞结构（Cytoarchitecture），是指在染色的脑组织中观察到的神经元的组织方式。
Brodmann分区最早由德国神经科医生科比尼安·布洛德曼（Korbinian Brodmann）提出。他的分区系统包括每个半球的52个区域。其中一些区域今天已经被细分，例如23区被分为23a和23b区等。
* 在线查询工具 http://www.fmriconsulting.com/brodmannconn/

### 哈佛脑模板（Harvard-oxford-atlases）基于结构像分割
* 基于MNI152空间体系；
* This probabilistic atlas is derived from 37 healthy young adult brains.
* It includes 48 cortical and 21 subcortical regions, as segmented from T1- weighted images
* It is kindly provided by the Harvard Center for Morphometric Analysis (CMA, Caviness et al., 1996)
* 官网 http://www.cma.mgh.harvard.edu/

## 基于结构连接
* 如基于DTI连接，较少用到，不再赘述，详细信息参考 http://web.mit.edu/fsl_v5.0.8/fsl/doc/wiki/Atlases.html

## 基于功能连接
* Power 2011
* 蒋田仔 2016
* 李志浩老师的丘脑分割模板


****
**References**
* AAL Automated Anatomical Labeling of Activations in SPM Using a Macroscopic Anatomical Parcellation of the MNI MRI Single-Subject Brain. N. Tzourio-Mazoyer, B. Landeau, D. Papathanassiou, F. Crivello, O. Étard, N. Delcroix, B. Mazoyer, and M. Joliot. NeuroImage 2002. 15 :273-289
http://dx.doi.org/10.1006/nimg.2001.0978
* AAL2 Rolls, E. T., et al. (2015). "Implementation of a new parcellation of the orbitofrontal cortex in the automated anatomical labeling atlas." NeuroImage 122: 1-5.
* Brodmann N. Tzourio-Mazoyer; B. Landeau; D. Papathanassiou; F. Crivello; O. Etard; N. Delcroix; Bernard Mazoyer & M. Joliot (January 2002). "Automated Anatomical Labeling of activations in SPM using a Macroscopic Anatomical Parcellation of the MNI MRI single-subject brain". NeuroImage. 15 (1): 273–289. doi:10.1006/nimg.2001.0978. PMID 11771995.
* 蒋田仔 Fan, L., et al. (2016). "The Human Brainnetome Atlas: A New Brain Atlas Based on Connectional Architecture." Cerebral Cortex 26(8): 3508.
* Power Power, J. D., et al. (2011). "Functional network organization of the human brain." Neuron 72(4): 665.
* 李志浩 Ji, B., et al. (2015). "Dynamic thalamus parcellation from resting-state fMRI data." Human Brain Mapping 37(3): 954-967.
* Evans, A. C., et al. (2012). "Brain templates and atlases." NeuroImage(1095-9572 (Electronic)).
