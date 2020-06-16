---
layout: post
title:  pyClusterROI
date:   2017-10-31 22:50:20
categories: toolbox
---


## Preparision
* toobox：Anaconda2 and pyClusterROI
* http://ccraddock.github.io/cluster_roi/usage.html
* https://github.com/ccraddock/cluster_roi
* First, you need to download the toolbox （see references）
* Second, ensure that you have installed the pre-requisites: NumPy，SciPy and NiBabel(只有nibabel需要额外安装，其他已经装好).
**Note: 务必在Anaconda prompt进行安装**

## Processing pipeline (demo)
* 第一步，功能像（rest or task state fmri data）数据预处理；
* 第二步，制作一个Gray matter mask，限制功能连接的空间范围；
* 第三步，利用数据在个体水平上构建相似矩阵（individual level similarity）。这个矩阵是一个Nvoxel x Nvoxel 的矩阵，矩阵的每个元素（entry）表示体素间的相似性。有三种方法可选：
        1. Tcorr，对应make_local_connectivity_tcorr **此函数有bug，line190，sparse_w=append(sparse_w,R[nzndx],1)应修改为 sparse_w=append(sparse_w,R[nzndx],0)**
        2. Scorr，对应make_local_connectivity_scorr
        3. 仅仅使用mask来制作，相邻的体素其相似性赋值为1，否则为0，对应函数为：make_local_connectivity_ones
* 第四步，聚类（clustering）。pyClusterROI提供了两种方法来整合不同被试的信息以实现组水平聚类。
        1. The group-mean clustering approach. 不需要个体水平聚类，只需要group_mean_binfile_parcellation函数；
        2. The two-level approach. 先在个体水平上聚类（individual level clustering），再在组水平聚类。需 binfile_parcellation import和group_binfile_parcellation函数。
* 第五步，将生成的npy格式文件转换为nii格式文件。有两个函数可以使用：
        1. make_image_from_bin_renum，有bug，不要用
        2. make_image_from_bin，ok        
```
from make_image_from_bin_renum import *
# write out for group mean clustering
for k in NUM_CLUSTERS:
    binfile='rm_group_mean_tcorr_cluster_'+str(k)+'.npy'
    imgfile='rm_group_mean_tcorr_cluster_'+str(k)+'.nii.gz'
    make_image_from_bin(imgfile,binfile,maskname)
```
* 第六步，给生成模板的每个ROI一个lable。即利用前人的模板，给分割的ROI命名。 注意这一步是通过在shell中调用函数 parcel_naming.py来进行的。
```
python parcel_naming.py tcorr05_2level_all.nii.gz \
    tcorr05_2level '50,100,150,200'
```
## adjust demo
**分为三步**
* 第一，获取指定文件夹下的文件名，参考 ：http://blog.csdn.net/lsq2902101015/article/details/51305825
* 第二，利用enumerate函数在for循环中计数。
   * enumerate多用于在for循环中得到计数（index=文件序号）和值（item=文件名）
   * enumerate(list1)返回两个参数，第一个是序号，第二个是文件名
```
test = ["this", "a", "test"] # def a var
for index, item in enumerate(list1): # 对于从enumerate函数返回的两个参数进行遍历
    print index, item # 按照index, item的顺序显示两个参数
```
* 第三，得到每个被试预处理fmri文件的信息：路径+文件名
该思路实施不便，因为函数make_local_connectivity_tcorr( infile, maskfile, outfile, thresh )，其输入infile定义为文件名，不包含路径信息。
* 第四，将所有被试的预处理文件重命名后整理到一个文件夹下。只遍历文件名信息。

******************
参考文献：A whole brain fMRI atlas generated via spatially constrained spectral clustering
