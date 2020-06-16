---
layout: post
title:  How to install FSL-AFNI on Ubuntu
date:   2017-10-20 16:10:20
categories: toolbox
---

**如果你使用的是Debian or Ubuntu系统，你可以使用neurodebian源（repository）轻松地安装FSL（默认联网）。**
具体步骤如下：
# 1.Add the neurodebian repository and update the sources.list
## 添加源
参考  http://neuro.debian.net/#how-to-use-this-repository

## 更新源列表（sources.list）
要想使用Debian/Ubuntu系统在线安装软件，首先需要配置source.list文件。source.lst文件位于"/etc/apt"目录下。
"source.list"编辑完成之后，使用下列命令使源生效。
```
 sudo apt-get update
```
# 2.安装具体的软件
## install FSL/AFNI (and related packages)
###  运行安装命令：
```
sudo apt-get install fsl fsl-doc fslview fslview-doc fsl-atlases fsl-possum-data fsl-first-data fsl-feeds dicomnifti libvtk5-dev vtk-examples vtk-doc
sudo apt-get install afni
```
**假如出现依赖报错，例如afni : Depends: libmotif4 but it is not installable，在apt-get命令中加入-f选项**
### 配置环境变量。
1. 使用下面第一行命令打开 .bashrc文件，然后把第二行的命令添加到系统的 .bashrc文件中。没有这一步，则无法从shell中使用fsl命令启动fsl。
```
# 需要先sudo su获得管理员权限才能保存
gedit ~/.bashrc
. /etc/fsl/fsl.sh # FSL
```
也可以使用下列命令来执行上述操作：
```
echo ". /etc/fsl/fsl.sh" >> ~/.bashrc
echo "export DISPLAY=localhost:0.0" >> ~/.bashrc
```
上述echo命令将下列语句添加到.bashrc文件中。其中第一行命令对于在shell中运行FSL是必不可缺的，第二行命令用于指定显示GUI的位置。
```
. /etc/fsl/fsl.sh
export DISPLAY=localhost:0.0
```
2. 但是之上命令运行完成之后，可以运行fsl命令。FSLDIR并没有添加到环境变量之后，运行``echo $FSLDIR``返回值为空。
按照官网教程，Debain/Ubuntu 安装完成之后，需要在 /etc/profile文件末尾输入
```
. /etc/fsl/5.0/fsl.sh
```
**AFNI安装完成之后，同样需要执行上述操作将“. /etc/afni/afni.sh”写入到bashrc文件中**

# 3.检查运行环境和软件命令
1. 重新启动PC；
2. shell中，输入``echo $FSLDIR``，应该返回正确的FSL安装目录。
3. type “fsl”，理应可以显示FSL界面，type“fslview”，理应可以显示FSLview界面。
4. afni同理

*****************
参考资料
* 官网： https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation/Linux
* http://miykael.github.io/nipype-beginner-s-guide/installation.html
* http://www.nemotos.net/?p=119
* http://neuro.debian.net/pkgs/afni.html for Discussion
