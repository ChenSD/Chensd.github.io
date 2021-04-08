---
layout: post
title:  Github 基础
date:   2017-10-12 23:10:20 +0800
categories: Programming
---
# 一、三种网页语言
## HTML
* （ HyperText Markup Language ）超文本标记语言，是一种用于定义网页每个组成成分的程式语言（侧重内容）。你可以利用它来定义网站中的文章内容、标题、连结、图片等，让浏览器知道网站整个架构的呈现。
* html代码都是成对出现的，一个标注开头，一个标注结尾。固定格式为：
`<html>
<head><title>网页标题 </title></head>
<body>文本内容  </body> # 所有输出的文字都被 <body></body>俩包围着。
</html>`

* <body> 元素定义了 HTML 文档的主体。
这个元素拥有一个开始标签 <body> 以及一个结束标签 </body>。
元素内容是另一个 HTML 元素（p 元素）。

## CSS
* CSS （Cascading Style Sheets）层叠样式表，是用于（增强）控制网页样式并允许将样式信息与网页内容分离的一种标记性语言。（侧重形式）。换句话说，它就是能为你的网站修改字体样式、颜色、网页背景、甚至是华丽的动画与 3D 效果、让你增添设计感的工具。
### HTLM和CSS
1. 区别。HTML是标记语言，用不同的标记对应不同的网页成分；CSS是样式语言，不包含任何标记。
2. 关系。 HTLM和CSS的关系：
   1. CSS代码可以写在HTML的标记内，可以集中写在HTML标记<style>中，也可写在其他标记中。如：<div style="width:100;"></div>
   2. CSS代码也可写在一个独立的文件内，然后被HTML的<LINK>标记调用。
   3. CSS可以定义很多HTML标记无法定义的效果。
## javascript
* javascript用以定义行为（动态网页？静态网页暂时不需要用到）。


# 二、Git的安装和配置

## Step1 安装软件

1. 安装git http://git-scm.com/downloads  
2. 安装一个支持markdown的文本编辑器，例如atom，https://atom.io/；

## Step2 Github和coding在线操作
部署博客的主机选择
* 1、github，[github在线博客库生成]( http://blog.csdn.net/wangyj1108/article/details/51444419)
* 2、coding


## Step3 配置SSH连接GitHub

* Github要求每次推送(push，本地推送到远程仓库)时候都要输入账号密码，用以验证你是否为合法用户。
* ssh是一种安全的传输模式. 为了省去每次都要输入密码的步骤，采用shh公钥,也就是sshkey来验证你是否为合法用户.
* 具体操作是在本地电脑生成了一个唯一的ssh公钥和私钥。公钥放到github上面，当你推送的时候，git就会验证你的私钥是否跟github上面的公钥相匹配——正确就认为你是合法的，允许推送。sshkey可以理解为是你的身份标识，放在github上面的项目内容别人是可以截获的，但是你本机的私钥别人就无法截获。这样sshkey就可以保证每次传输都是安全的。

* **教程** [配置SSH连接GitHub ](http://jingyan.baidu.com/article/a65957f4e91ccf24e77f9b11.html)

## Step4 本地初始化（init）

1. 建立本地分支 (master)。
```
git init
git clone + 远程仓库地址
```   
2. 注意，Github默认使用了https方式来push代码，这样会导致每次push的时候都需要输入用户名和密码。虽然可以在~/.netrc文件里设定用户名密码，不过这样的风险在于密码是明文存放在这个文件里的，比较容易泄露。如果我们改为SSH方式就可以避免该问题。所以最好在克隆的时候在github上选择ssh(clone的地址下面有)对应的URL。

3. http2SSH。 如果是已经克隆好的只需要修改config中remote的存储信息。为此在shell终端 输入

`` Git remote -v ``

可以看到形如以下的返回结果

origin    https://github.com/username.git (fetch)
origin    https://github.com/username.git (push)

现在把它换成ssh方式就可以了  
先删除：git remote rm [name]
再重新添加：git remote add origin [name]

此时我们看到https已经改为了SSH方式
origin    git@github.com:username.git (fetch)
origin    git@github.com:username.git (push


## Step3 选择什么静态网页生成工具？
  具体使用什么工具来生成本地的静态网页并且推送到github？  

  1. amWiki. 最简单！
   http://www.qdfuns.com/notes/30959/43d2abc7d3aedcd32b983347f9572af6.html  

   需要注意的是，amwiki 可以同时在 Atom 编辑器和 node.js npm 的命令行两个平台工作，即既可以作为atom的一个插件运行，也可以作为nodejs的一个全局模块运行。

  2. Hexo. 最复杂！
  hexo是一款基于Node.js的静态博客框架。与amwiki的区别在于，hexo只向远程仓库推送本地生成的静态网页，源文件并不推送。所以设置起来很麻烦。

  3. jekyll。**推荐！！** 和amwiki类似，比amwiki复杂一点，优点是有较多的主题可以选择。不像amwili只能默认主题。jekyll的原理是把源文件上传到github，由github生成网页，所以跨电脑写作很方便，不用像hexo一样操碎了心。

# 三、 Github/Coding 基本术语

## 1. 基本存储单位：仓库（repository）
存储代码的结构，不同于常见的windows目录只存储保存的文件，其还记录对于代码的各种操作（仅限于文本文件），例如增加、删除、修改等等，以便后来跟踪、修改或还原。

### 远程仓库（remote），远程仓库一般命名为源（origin）。
*  查看当前remote: git remote -v
   如果没有需要添加remote：git remote add [name] [url]，添加后存储在git/config文件中。  
*  定义origin：git remote add origin [url]  
*  删除远程仓库：git remote rm [name] 如 git remote rm origin 删除远程的origin连接  
*  修改远程仓库：`git remote set-url --push [name] [newUrl]`
*  拉取远程仓库到本地： `git pull [remoteName] [localBranchName]`
*  推送本地修改到远程仓库：`git push [remoteName] [localBranchName]`
   *如果定义好了origin（本地仓库一般默认为master），则可以写为：git push origin master*

### 分支仓库：master（主分支）+ 其他分支（branch）

*  查看本地分支：git branch  
*  查看远程分支：git branch -r  
*  创建本地分支：git branch [name]


## 2. 工作流（Workflow），推送和下载

### 推送本地更新到远程仓库（Push）
* step1: git add [filename] or git add .  
将工作目录下的文件提交到暂存区（stage area）。暂存区的用来准备一个提交，但可以不用把工作目录中所有的修改内容都包含进来。    
git status # 查看本地仓库的修改状态

* Step2: git commit -m "代码提交信息"
将改动提交到HEAD，但是还没有到远程仓库。
HEAD是一个指向t当前版本的指针。  
通过改变HEAD的指向，我们可以退回到历史中的某个版本。  
git log命令显示最近的提交日志，显示顺序为最近到最远。

*请记住，因为前两步的命令只在本地运行，我们可以按自己需求反复操作多次，而不用担心远程仓库上有了什么操作。*

* Step3：git push origin master
  将HEAD中的改动提交到远程仓库。可以把 master 换成你想要推送的任何分支。
  ***
  > 如果push失败，输入 git push origin master
   提示出错信息：error:failed to push som refs to .......
   解决办法如下：  
   1、先输入$ git pull origin master //先把远程服务器github上面的文件拉下来进行合并  
   2、再输入$ git push origin master
   ***
   > 如果pull失败
   提示出错信息： You have not concluded your merge (MERGE_HEAD exists)
   可能是因为在你以前pull下来的代码没有自动合并导致的.   
   有2个解决办法:
   1.保留你本地的修改
   git merge --abort
   git reset --merge  
   合并后记得一定要提交（push）这个本地的合并, 然后再获取线上仓库  
   git pull  
   2.下载线上代码版本,抛弃本地的修改
   不建议这样做,但是如果你本地修改不大,或者自己有一份备份留存,可以直接用线上最新版本覆盖到本地  
   git fetch --all
   git reset --hard origin/master
   git fetch

### 下载远程仓库内容到本地（Pull）
要更新你的本地仓库至最新改动，执行：
git pull
以在你的工作目录中 获取（fetch） 并 合并（merge） 远端的改动。
要合并其他分支到你的当前分支（例如 master），执行：
git merge <branch>
在这两种情况下，git 都会尝试去自动合并改动。遗憾的是，这可能并非每次都成功，并可能出现冲突（conflicts）。 这时候就需要你修改这些文件来手动合并这些冲突（conflicts）。改完之后，你需要执行如下命令以将它们标记为合并成功：
git add <filename>
在合并改动之前，你可以使用如下命令预览差异：
git diff <source_branch> <target_branch>

# 四、Git多人协作

  参考教程： https://www.cnblogs.com/zhaoyanjun/p/5882784.html
  一句话，就是需要一个人建立一个团队，担任队长。队长先邀请队员入队，然后给队员分配读写权限。然后队员就可以读写push了。

# Git突然连接不上time_out

inside the .ssh folder Create "config" file

``Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443

Host gitlab.com
Hostname altssh.gitlab.com
User git
Port 443
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa``
