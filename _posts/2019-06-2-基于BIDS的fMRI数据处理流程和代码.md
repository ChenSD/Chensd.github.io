---
layout: post
title:   基于BIDS的fMRI数据处理流程和代码
date:   2019-06-2 22:10:20
categories: toolbox
---

# 数据采集-整理和分析

## fMRI data

* 使用AFNI调整和优化参数（主要以TSNR和SNR为指标）
* 数据采集过程中，就可以通过肉眼观察剔除头动过大等原因造成的无效数据；
然后及时把删掉的被试补回来。
* 整理有效被试名单
* 本实验有两大任务，两个任务需要分开处理。
* 把原始数据整理为BIDS形式。

### 最终参数

1. 20-channel coil
2. SMS-2
3. TR 1.5, flip angle=77
4. FOV=196, 64*64*44, voxel size=3*3*3

## 使用行为数据建立timing files

* Timing files（TF）的作用是同步行为数据的刺激和fMRI数据的TR，本质是每个刺激出现的时间。
* SPM的规则是每个被试每个run建立一个TF，文件格式为：条件*时间。Each row is associated with a condition.
* AFNI的规则是每个被试的每个条件建立一个TF：run*时间。Each row is associated with a run.
* 根据行为数据，计算出每个被试每个条件的timing files。
* 任务1只有1个条件（被试间变量每组只有1个水平），任务2有两个条件。

## Plans of Data analysis

### 1. Preprocessing and first-level analysis

1. 预处理，所有被试一起分析；注意预处理方法（ComCorr/AROMA）的选择和质量控制。
2. 个体分析。实验设计为：组间*前后测。前后测的数据分开处理，建立两个文件夹（pre/post）；
由于被试编号是两组统一编号，所以组间不用区分，同一任务所有被试放在一个文件夹下。


### 2. Group-level Univariate Analysis

1. 前测-Task1。两组的感兴趣条件（文字或面孔）做独立样本t-test。
2. 后测-Task2。两组的感兴趣条件（面孔）做独立样本t-test。问题：是先在组内求出每个被试neg-neu的差值，
再在组间比较差值，还是直接比较两组的neg？
3. [组分析结果的查看和呈现](http://www.bobspunt.com/software/bspmview/)

### 3. Correlation analysis

1. 前测任务1内，试次和调节效果的相关分析。这里为了排除习惯化或疲劳效应，应该分别计算两组
每一个被试任务1内，试次和调节效果的相关系数。然后针对相关系数在组间进行独立样本t-test。
2. 为了检验两组之间的脑功能是否存在关联，进行任务1和任务2的相关分析。
使用组间激活差异的脑区信号，或者差异显著的脑网络指标，预测任务2的情绪诱发指标。

### 4. Advanced multivariate fMRI analysis

1. 脑网络分析。比较任务1，两组被试的脑网络拓扑属性是否存在差异。
2. 高级分析，MVPA AND RSA

# BIDS

## What and why is BIDS?

* What BIDS ? BRAIN IMAGING DATA STRUCTURE (BIDS)是一个目前非常流行的组织磁共振数据的标准(Gorgolewski et al., 2016).。
* ![BIDS架构](/images/19-6-2/2019-06-03_161748.jpg)
* [Why BIDS?](https://rpubs.com/sarenseeley/bids-fmriprep-mriqc)

## How convert raw dicom data to BIDS nii files?

Use the toolbox [Dcm2Bids](https://github.com/cbedetti/Dcm2Bids)

1. Install: ``pip install dcm2bids``
2. Usage:
    1. cd <YOUR_FUTURE_BIDS_FOLDER>
    2. Build your configuration file with the help of the content of [tmp_dcm2bids/helper](https://cbedetti.github.io/Dcm2Bids/tutorial/#building-the-configuration-file)     	
	3. ``dcm2bids -d DICOM_DIR -p {01..60} -c CONFIG_FILE``
3. Run the bids-validator to check your directory.

## BIDS APPs

1. Preprocessing: fmriprep
2. Univariate/Voxel-vise analysis based on SPM：SPM BIDS App, CONN (ToolsImport.FromBIDS)
3. Multivariate analysis [BrainIAK ORG](https://brainiak.org/) [BrainIAK Github](https://github.com/brainiak/brainiak)
 BrainIAK tutorials: user-friendly learning materials for advanced fMRI analysis


# fMRI预处理

## 0. 搭建环境 fmriprep
1. [安装docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
2. [Docker 创建docker用户组，应用用户加入docker组](https://blog.csdn.net/point0mine/article/details/79448402)
install docter and add the current user to the docker group. install  ``pip install --user --upgrade fmriprep-docker``
3. [Add fmriprep path to your PATH environment variable](https://neurostars.org/t/fmriprep-docker-command/4105/6)

## 1. 将数据整理为BIDS格式


## 2. 使用MRIQC检查数据质量，剔除极端数据

1. [MRIQC](https://mriqc.readthedocs.io/en/stable/docker.html#docker)
2. 注意，MRIQC目前只能处理未经预处理的原始数据。所以只能先运行MRIQC，
再运行fmriprep
3. ``sudo bash command``
4. 如果不指定participant-label，MRICQC默认会生成所有被试的质量报告（html和tcv文件）。
5. 如果运行报错，重启docker
6. [主要指标FD>0.2mm](https://mriqc.readthedocs.io/en/stable/iqms/bold.html) (Power 2013)
7. 如果被试数目太多，可能数据运算量会超过工作目录大小，就会报错。可以分批处理。

## 3. Preprocess by fmriprep

1. [Why fMRIPrep ?](https://rpubs.com/sarenseeley/bids-fmriprep-mriqc))
2. [fMRIPrep tutorial](https://dartbrains.org/features/notebooks/8_fmriprep_tutorial.html)
3. [Neurostars the form to post questions and learn from others](https://neurostars.org/)
4.  
```fmriprep-docker /input_bids_dir /output_dir participant
--participant-label 01 --fs-license-file /data/fMRI/freesurfer_license.txt --ignore slicetiming```

### 必选项

1. input dir 输入文件夹
2. output_dir 输出文件夹。设置为输入文件夹所在的根目录即可，fmriprep会把所有被试预处理文件放在其生存的fmriprep文件夹中。
3. participant 默认选项
4. participant-label 被试编号，为sub-后面的部分，例如01，02.
5. freesurfer liscense

### 常用选项

1. --work-dir or -w  存储中间文件的路径

2. --use-aroma option  will internally smooth the data in the same way as described in the original ICA-AROMA paper (6mm FWHM)((https://doi.org/10.1016/j.neuroimage.2015.02.064 ). Most outputs will stay unsmoothed, however the *bold_space-<space>_variant-smoothAROMAnonaggr_preproc.nii.gz files will be smoothed.
Note that if you use the variant-AROMAnonaggr version of the outputs, then you will not want to use the Aggressive AROMA regressors nor other motion regressors in your modeling. Otherwise, you risk re-introducing noise. As you mention, if you use variant-AROMAnonaggr then do not apply any additional spatial filtering (as SUSAN has been applied). No temporal filtering is performed on the variant-AROMAnonaggr outputs (therefore, you’ll need to insert that step in your analysis workflows)(https://neurostars.org/t/fmriprep-smoothing-option/259/14). If your modeling is designed to use such regressors, then you shouldn’t pick the variant-AROMAnonaggr outputs
Note that due to ICA-AROMA being run MNI152NLin6 will be automatically added.

3. --use-syn-sdc: “In the absence of direct measurements of fieldmap data,
we provide an (experimental) option to estimate the susceptibility distortion
based on the [ANTs symmetric normalization (SyN) technique](https://rpubs.com/sarenseeley/bids-fmriprep-mriqc).
This feature may be enabled, using the –use-syn-sdc flag, and will only be applied if fieldmaps are unavailable.”

4. --ignore slicetiming option disable slice timing

5. --fs-no-reconall option disable FreeSurfer surface preprocessing.

6. --output-spaces {MNI152NLin2009cAsym} 设置MNI152NLin2009cAsym为标准化空间。

### fmriprep results (Html)

#### 1. [Anatomical Scans](https://fmriprep.readthedocs.io/en/1.1.0/outputs.html)

1. T1 Segmentation: region of interest (gray CSF and white matter)
and of no interest ( white matter/WM and cerebral spinal fluid-filled spaces/CSF)
2. T1 to MNI Registration

#### 2. Functional Scans

1. Skull stripped EPI
2. EPI to T1 Registration
3. aCompCor Mask. CSF and white matter ROIs were combined to form the anatomical noise ROI.
The aCompCor means the application of CompCor with the anatomical noise ROI (Behzadi et al., 2007).
用以指定白质和脑脊液信号的范围，并在个体阶段使用回归模型消除其干扰。[使用aCompCor相比其他方法能更好地降低头动影响](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4043948/)
4. [Carpet Plots ](https://www.sciencedirect.com/science/article/pii/S1053811916303871?via%3Dihub)

### 3. Which output files of fmriprep should be used in first-level GLM?

这里又具体分两种情况，第一是不使用AROMA方法去噪的，第二是选择使用AROMA方法的。

#### 不使用AROMA

1. Signal: *space-MNI152NLin2009cAsym_preproc_bold.nii.gz
配准到MNI152NLin2009cAsym模板，没有平滑的4D功能像数据
2. Noise Covariates: * desc-confounds_regressors.tsv文件中包括头动参数（6个），以及根据aCompCor得到的CSF, WM 还有CSF-WM combined mask内的信号。
[这些信号需要在个体或组分析中进行回归以进一步消除其影响](https://neurostars.org/t/confounds-from-fmriprep-which-one-would-you-use-for-glm/326/6)。
    1. Confounds on 1st level: 6 motion parameters (trans_xyz平动3个方向,rot_xyz转动3个方向),
	[Frame-Wise Displacement(FD)](https://dartbrains.org/features/notebooks/10_GLM_Single_Subject_Model.html), and aCompCor。**！！！[When using AROMA-denoised data (*_bold_space-MNI152NLin2009cAsym_variant-smoothAROMAnonaggr_preproc.nii files),
	you would likely NOT want to regress out motion-related variables from confounds.tsv as this may reintroduce motion.](https://sarenseeley.github.io/BIDS-fmriprep-MRIQC.html#aroma) **
    2. Confunds on group level: mean FD(for both task and rest).
3. [Discussion](https://neurostars.org/t/fmriprep-which-regressors-for-best-global-signal-regression-comparison/1878/2)

### 使用AROMA方法

1. Signal: *space-MNI152NLin2009cAsym_desc-smoothAROMAnonaggr_bold.nii.gz
配准到MNI152NLin2009cAsym模板，smooth并使用AROMA方法进行了降噪（主要是头动噪声？）后的4D功能像数据。
2. Noise: *desc-confounds_regressors.tsv [注意，因为AROMA已经利用ICA的方法去除了头动噪声，GLM中不再需要加入头动参数，只需要CSF,WM的信号以及线性趋势的信号即可](https://sarenseeley.github.io/BIDS-fmriprep-MRIQC.html#aroma)。
3. 为什么CSF,WM的信号可以表征噪声？Since neural activation is localized to gray matter,
white matter and CSF regions should primarily reflect signals of non-neural origin, such as
cardiac and respiratory fluctuations. (Behzadi et al., 2007).
4. 具体指标： *desc-confounds_regressors.tsv中a_comp_cor_00到a_comp_cor_04，即aCompCor的前五个成分。
fmriprep生成的confounds文件中，aCompCor的成分是按照贡献大小从高到低排列的。
所以前五个成分贡献了大部分的噪声？？？Determining the optimal number of PCs is an open question (e.g. fixed number vs. %of variance)

### 区分ComCorr和AROMA两种降噪方法

1. ComCorr（PCA方法）针对的主要是cardiac and respiratory related-noises；
2. AROMA（ICA方法）针对的主要是motion artifacts. "In contrast to generic
ICA-based denoising strategies (Salimi-Khorshidi et al., 2014), we here
focus specifically on the classification and removal of components that
specifically relate to head motion. (Raimon H.R. Pruim 2015)"




# 1st by SPM12

### fMRI model specification

此处仅讨论有争议的几个选项，其余参考SPM12 manual。

1. Microtime resolution. 对于功能像的EPI序列，一个TR内的全脑图像是分层扫描的。例如一个TR为2S，
一个全脑分为20层，逐层扫描，那么相当于0.1秒扫1层。这个0.1/层就是所谓的time-bins。一个TR内time-bins的
总数目称为microtime resolution。原理上讲这个应该设为层数，但是SPM建议除非TR较长，默认即可。
2. 当SPM创建信号的GLM模型时，它需要知道数据扫描的具体时间。Microtime onset意味着一个TR扫描开始时，
最先扫描那一层的onset time. 如果你做了Slice timing，
那么“microtime onset”就应当是扫描中间一层时的时间。
但是注意，这里填的是“中间层”的层数。
3. 对于当前数据，一个TR内采集了44层；而且由于采用了多层采集，预处理时没有进行slice timing，
所以这里Microtime resolution可以（注意是“可以”，而非“应该”）填44，microtime onset填1.


###  Tips

1. 关于头动。传统的方法是在预处理头动配准的阶段，SPM会生成每个被试在6个方向上的空间位移，保存为
一个rp*开头的txt文件。然后在个体分析的时候，为了消除头动对信号的干扰，
我们会把这6个头动参数作为协变量加入到GLM中。
在使用统计模型去除头动影响时，可以选择[WLS](http://www.diedrichsenlab.org/imaging/robustWLS_spm12.html)
2. 但是如果使用fmriprep所带的AROMA方法去除头动的话，后续就无需再加入头动参数！

### 结果

1. Parameter estimates also called Beta values (β= treatment effects in
ANOVA, or the slope of the regression line=con*.nii files)
2. β values represent partial correlations between predictor and the observed
data
3. β values can be thought of as scaling the regression function to give the
best fit to the data

### [个体分析结果的检查](http://www.bobspunt.com/resources/teaching/single-subject-analysis/)。SPM/Design，检查项目包括：

1. 时域（Time domain）感兴趣条件的协变量的模拟信号是否存在重叠现象
2. 频域（frequency domain）感兴趣条件的频率是否健在？是否受到预处理denosing技术和
	一阶分析高通滤波的影响？
3. 独立性-Design Orthogonality。不同的感兴趣条件之间的正交矩阵能否区分开不同的条件？
如果条件之间存在高度的相关（black），那么你就无法从统计上把两者区分开！

### 多重检验校正方法的选择

1. DAPABI推荐Permutation test with Threshold-Free Cluster Enhancement (TFCE)
2. [SPM有专门进行TFCE的工具包](https://www.fil.ion.ucl.ac.uk/spm/ext/)
3. [SPM TFCE](http://www.neuro.uni-jena.de/tfce/)
此工具包还可以加载额外的mask进行small volume correction。
Please keep in mind that the mask image has the same dimensions such as your data.
4. [使用完TFCE后，依然要用FWE或FDR对置换检验的结果进行多重检验校正。两种方法都可以使用。](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=spm;206ac0f8.1310)
5. TFCE-Results界面打开结果后，会给出使用type of statistic: TFCE or T?"。选择后者的话，结果和SPM原始结果基本一样。
所以这里应该选择TFCE statistic/FWE.

# 2st analysis by SPM12

## FAQ and Tips

1. 使用FSL进行一阶分析时，因为涉及反卷积计算，耗时甚多。[官方论坛推荐使用OpenBLAS多线程加速解决这一问题](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=FSL;4f8a92e.1902)。
``export OPENBLAS_NUM_THREADS=2``
2. [AFNI trial by trail GLM](https://afni.nimh.nih.gov/afni/community/board/read.php?1,155769,155769)


# 预处理步骤和高级分析

1. ICA的数据，最好不要做去平滑和滤波，[see GIFT email](https://sourceforge.net/p/icatb/mailman/message/28057801/?tdsourcetag=s_pctim_aiomsg)
2. MVPA的数据，最好不要做平滑，可以做微平滑以提高信噪比（例如2*2*2）
3. 功能连接的数据，CONN会默认在Denosing面板中进行带通滤波和Detrending。

# Advanced Analysis

## Independent Variables：ICA




## Dependent variables

1. 指标1：行为数据，情绪主观评分
2. 指标2：杏仁核-（梭状回-面孔加工）时间序列
3. 指标3：杏仁核-MPDC功能连接
4. 指标4：任务态全脑网络（power模板）图论属性
5. 指标5： MVPA分类器正确率（浪费时间，效率低）

## 回归方法

1. 多元回归分析：纳入10个网络？每个网络的贡献率在两组之间的差异？雷达图？

# References
1. BIDS https://bids.neuroimaging.io/
2. Gorgolewski, K. J., Auer, T., Calhoun, V. D., Craddock, R. C., Das, S., Duff, E. P., ... & Handwerker, D. A. (2016). The brain imaging data structure, a format for organizing and describing outputs of neuroimaging experiments. Scientific Data, 3, 160044.
3. Nichols, T. E., Das, S., Eickhoff, S. B., Evans, A. C., Glatard, T., Hanke, M., ... & Proal, E. (2017). Best practices in data analysis and sharing in neuroimaging using MRI. Nature neuroscience, 20(3), 299.
4. Gorgolewski, K. J., Alfaro-Almagro, F., Auer, T., Bellec, P., Capotă, M., Chakravarty, M. M., ... & Eklund, A. (2017). BIDS apps: Improving ease of use, accessibility, and reproducibility of neuroimaging data analysis methods. PLoS computational biology, 13(3), e1005209.
5. [夏晓凯BIDS数据处理-基于windows系统](https://github.com/dddd1007/NfMRIx)
6. [CONN compatibility with BIDS and fmriprep output](https://www.nitrc.org/forum/message.php?msg_id=26277)
7. Behzadi, Y., et al. (2007). "A component based noise correction method (CompCor) for BOLD and perfusion based fMRI." NeuroImage 37(1): 90-101.
8. [Getting started with BIDS, fMRIPrep, MRIQC](https://sarenseeley.github.io/BIDS-fmriprep-MRIQC.html#fmriprep)
