---
layout: post
title:   数据处理的利器HPC和Singularity
date:   2019-06-6 22:10:20
categories: toolbox
---

# What HPC and Container technology (Docker and Singularity)

1. HPC, HPC是高性能计算(High Performance Computing)机群的简称。
相比个人PC，HPC可以并行处理大数据，处理速度和个人PC也不可同日而语。
2. Container technology, 容器技术。软件包的本质是一系列的函数包，然后函数之间乃至软件包之间的存在相互依赖的关系，而且每个依赖库都有特殊的版本需求。
这种依赖关系导致每个使用软件的个体都需要自己配置相应的环境，大大增加了物力人力，降低了软件的可流通性。
容器化技术就是把所有该软件所需的环境打包成一个单独的包，我们仅仅需要配置运行容器的环境就可以。
3. 常见的Container technology：如Docker和Singularity。
Singularity是在劳伦斯伯克利国家实验室专门为HPC和深度学习(DL)工作负载开发的。研究团队发现与裸机相比，在Singularit上的DL应用程序没有性能损失。
4. [Docker和Singularity的差别](http://psg.skinforum.org/blogger_container_hpc.html)
    * 有的超算（即HPC）不支持Docker，只能配置Singularity；
	* Singularity可以完全独立于docker工作；可以导入docker image文件，将其转化为singularity images, 或者直接运行docker容器。
	* Docker更适用于开发者（developers），Singularity更适合科学工作者（Scientific dogs）
	* Dockers更适用于小程序、微服务开发，Singularity适用于具有复杂依赖环境的工作流畅（fmriprep is a good example!!!）
	* Docker比虚拟机VM快得多，而Singularity比Docker快一点。
	* Docker创建流程：本地生存docker images，上传到Docker Hub（云端或私人云），用户端联网下载和调用。
	* 各大高校都用Singularity（那是因为这些高校有HCP啊，但是我们没有····）
5. [singularity安装创建和运行](https://www.jianshu.com/p/5b5e2f1bd057)

# BIDS APP

* 基于BIDS，科研雷锋们开发了许多BIDS APP。
* A BIDS App is a container image capturing a neuroimaging pipeline that takes a BIDS-formatted dataset as input. Since the input is a whole dataset, apps are able to combine multiple
modalities, sessions, and/or subjects, but at the same time need to implement ways to query
input datasets (Gorgolewski, K. J., et al., 2017).
* BIDS Apps主要基于Docker（for本地电脑或云端）和Singularity（for HCP）两种容器化技术来制作工具包。
* [BIDS APPs](https://github.com/BIDS-Apps)
* 可以在docker hub上查询这些APP的docker镜像

## BIDS APPs for HCP

* 使用sbatch来提交作业

# References
1. Gorgolewski, K. J., et al. (2017). "BIDS apps: Improving ease of use, accessibility, and reproducibility of neuroimaging data analysis methods." PLoS Computational Biology 13(3): e1005209.
