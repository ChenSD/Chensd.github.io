---
layout: post
title:  交互式文档Jupyter and knitr
date:   2018-04-19 22:50:20
categories: Programming
---


# 交互式文档
* 什么是交互式文档？就是代码和笔记的混合体！相比单纯的笔记，和单纯的代码，交互式文档的优点在于：
   1. 能够在浏览器编辑代码，这边浏览copu，另一边就能测试学习，无需在软件之间来回切换；
   2. 交互式编程, 代码可以立刻运行得到可见结果；
   3. 可以添加各种元素,比如图片,视频, 链接, 文档(比代码注释要好看), 相当于PPT

# Jupyter Notebook
* [Jupyter Notebook](http://jupyter.org/)是一个典型的交互式笔记本。
* 为什么选择Jupyter Notebook？
   1. Python, R and Shell···支持运行 40 多种编程语言，几乎全能。
   2. 好看
* 参考教程： https://zhuanlan.zhihu.com/p/28704300
* 注意，安装R Kernel的时候，devtools::install_github('IRkernel/IRkernel')会一直timeout而失败。解决办法是先在github下载IRkernel安装包，然后运行：  
`devtools::install("C:\\Users\\ecolla\\Downloads\\IRkernel-master\\IRkernel-master")`  
注意双斜杠！
* 可通过设置config文件的方法来设置固定的工作路径。方法是：
   1. 选择一个用于存放config文件的文件夹
   2.  在cmd中进入该文件夹的路径
   3. 在cmd中 输入​命令jupyter notebook --generate-config
   4. 此时在该文件夹中便生成一个notebook的config文件​，文件名是“jupyter_notebook_config.py”
   5. 打开该文件，修改“# The directory to use for notebooks and kernels.”下面的 “# c.NotebookApp.notebook_dir = ””​为“c.NotebookApp.notebook_dir = ‘指定的工作路径’”​（注意将#号删除）
* 配置文件位置 C:\Users\用户名.jupyter\jupyter_notebook_config.py


# R knitr
* knitr by 谢益辉
   * knitr允许在文本中插入代码模块（code chunks），这些代码模块在输出时可以直接运行；
   * 代码模块运行的结果（文字，图表乃至警告，报错）默认输出到最终的报告文档；
   * knitr除了可以直接插入代码模块，也可以直接调用外部的语句；
   * knitr支持各种编程语言的输入，如Python和R，也支持各种标记语言的输出（e.g. LaTex, Markdown）
* [liftr](https://liftr.me/) by Nan Xiao

## How to use knitr

1. 安装：
  * `install.packages('knitr', dependencies = TRUE)`
  * 为了编译pdf，windows系统需要安装Miktex，Linxu系统需要安装Tex Live 2013+
2. 创建R-Markdown文档:在Rstudio中，点击File -> New File -> R Markdown。MD的类型有：
   1. Document，文档。文档的格式又有三种：HTML，pdf和WORD。默认选择HTML。
   2. Presentation，生成PPT；
   3. Shiny，生成交互式文档
   4. From Template, 根据网上下载的模板生成文档  
选择类型之后，填写标题和作者，点击"OK"。
3. 在文档中插入文版或代码块。可以通过绿色的Insert按钮插入各种代码块。每个代码块都可以单独运行，并即时显示结果。
4. 点击上方工作栏中的knit按钮，进行编译。所有代码块的结果都会进行编译，输出到最终的报告文档中。


参考教程：
1. https://www.cnblogs.com/nxld/p/6059603.html
