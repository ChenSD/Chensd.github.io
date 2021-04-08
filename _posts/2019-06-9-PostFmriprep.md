---
layout: post
title:   PostfMRIprocessing
date:   2019-06-9 22:10:20
categories: toolbox
---

## FSL

1. https://github.com/poldrack/fmri-analysis-vm/tree/master/analysis/postFMRIPREPmodelling
2. https://github.com/poldracklab/fitlins
3. https://mumfordbrainstats.tumblr.com/post/166054797696/feat-registration-workaround
4. [an example Nipype workflow for post-fMRIPrep first- and second-level models in FSL](https://github.com/poldracklab/ds003-post-fMRIPrep-analysis)

# Nipype

1. [Nipype org](https://nipype.readthedocs.io/en/latest/)
2. [Nipype manual](http://miykael.github.io/nipype-beginner-s-guide/)
3. [NiBabel](https://nipy.org/nibabel/) nii数据读取

## Install

1. 安装Neurodebian。注意密钥按照官网教程无法安装，使用下列命令可以：
``sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2649A5A9 ``
2. 安装conda
3. conda install nipype
4. 如果遇到permission error，sudo chmod -R 777

## 1st

1. 视频教程

https://youtu.be/Nkt6hsY-T0s
https://youtu.be/GncNH7l6OOQ


2. [codes](https://github.com/arash-ash/nipype_tutorial)

3. [Adding additional regressors to SPM modelgen with nipype](https://neurostars.org/t/adding-additional-regressors-to-spm-modelgen-with-nipype/1089/3)
4. [Nipype: SPM realignment multiple sessions](https://neurostars.org/t/nipype-spm-realignment-multiple-sessions/204)



# SPM BIDS APP 测试

之前的[aa](http://ssdd.site/toolbox/2017/11/01/aa-toolbox/#11-%E5%9F%BA%E6%9C%AC%E7%9F%A5%E8%AF%86xml%E8%AF%AD%E8%A8%80)工具包已经过时了。aa工具包的参数配置没有一个很好的说明文档，配置起来很头疼。
aa的优点就是分离了配置参数和运行命令，而如今基于BIDS的SPM BIDS App也实现了这种功能。
两者的差别在于，aa使用了xml语言来配置参数文件，而SPM BIDS App用的是matlab语言。
相比之下，matlab语言可比xml简单多了。
类似于fmriprep，SPM BIDS App的运行也分为以下几步

1. 如果没有安装，docker pull bids/SPM
2. [usage](https://github.com/BIDS-Apps/aa): run <bids_dir> <output_dir> {participant|group}
           [--participant_label <participant_label>]
           [--freesurfer_license <license_file>]
           [--connection <pipeline to connect to>]
           [<tasklist> <user_customisation>]
3. docker run -ti --rm \
  -v /tmp:/tmp \
  -v /var/tmp:/var/tmp \
  -v /path/to/local/bids/input/dataset/:/data \
  -v /path/to/local/output/:/output \
  -v /path/to/local/cfg/:/cfg \
  bids/spm \
  /data /output participant \
  --participant_label 01    \
  --config /cfg/my_pipeline_1st.m
4. 命令详解：
    -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用（默认）；
	-i: 以交互模式运行容器，通常与 -t 同时使用（默认）；
	--rm自动移除已经存在的容器（断开联系而非删除容器本身）（默认）；
	--volume/-v The -v flag mounts the current working directory into the container（需要修改）.
5. [BIDS Tools in SPM](https://en.wikibooks.org/wiki/SPM/BIDS)

## SPM BIDS APP 测试感受 (baybay6)

1. The biggest problem is that SPM is not supported of compressed NIfTI files (.nii.gz),
 thus as well as SPM BIDS APP.
2. [spm_select](https://en.m.wikibooks.org/wiki/SPM/Working_with_4D_data)函数不支持导入nii.gz格式的4D数据，只支持nii格式；
3. [Convert nii.gz to nii](https://en.wikibooks.org/wiki/SPM/BIDS)
    1.  ``gunzip('*.gz')``
	2. if you encounter a problem "could not open file", try ``sudo chmod -R 777 /data/fMRI/fmriprep``


# References

1. https://neurostars.org/t/performing-a-basic-two-sample-t-test-in-spm-using-nipype-interfaces-spm/493
2. [python script to load confounds](https://neurostars.org/t/confounds-from-fmriprep-which-one-would-you-use-for-glm/326/32)
