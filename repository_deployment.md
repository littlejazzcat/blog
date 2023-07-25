# **添加远程仓库以及关于git的一点疑问**

- [1.如何克隆远程仓库到本地](#1)
- [2.fetch & push](#2)
- [3.在github主页面部署密钥和在仓库设置中部署的区别](#3)

---

 <i> <font color=#FF0000> 在添加远程仓库前要先确保以完成ssh部署，具体参考这篇文章</font>[github ssh部署记录](https://www.cnblogs.com/littlejazzcat/p/17578723.html) </i>
 <br>

<h3 id ="1">1.如何克隆远程仓库到本地</h3>

- 首先获取远程仓库的URL<br>
  (1)打开github上自己想要添加的远程仓库<br>
  (2)点击绿色的code按钮<br>
  (3)选择ssh选项（我这里用的ssh部署）<br>
  (4)复制URL<br>

    <i>可以参考下图 </i>

![获取远程仓库的URL](https://pics0.baidu.com/feed/314e251f95cad1c890509dd8d8ade005cb3d5181.jpeg?token=22753cc899217a02c1bfbdc83bb58546)

- 将本地目录初始化成git可管理的仓库<br>
  在gitbash终端键入命令<code>git init</code>
  <br>
  <em>注意要在你要当成本地仓库的路径下（也就是你准备存放源代码等文件的地方）执行。<font color=#FF0000>如果跳过了这步则会在添加远程仓库时显示：<code>fatal: Not a git repository (or any of the parent directories): .git</code></font></em>
  <br>
- 添加远程仓库的URL<br>[](#remote)
  在gitbash终端键入命令 <code>git remote add origin  git@github.com:自己的GitHub账户名/自己的仓库名.git </code>
  <br>
  <em> origin是远程库的名称（你也可以换成其他的），origin后面的是前面复制的URL </em>
  <br>
- 查看已添加的远程仓库信息<br>
  在gitbash终端键入命令 <code>git remote -v</code> <br>
  <i>每个仓库都会有两个URL如下所示：</i>
  <br>
  > origin  https://github.com/OWNER/REPOSITORY.git (fetch) <br>
  > origin  https://github.com/OWNER/REPOSITORY.git (push) <br>
<em>前面的origin是（默认的）仓库名称，后面的URL带有(fetch)的是拉取远程仓库数据的URL，带有(push)的是向远程仓库推送的URL。"(fetch)"和"(push)"分别表示从远程仓库拉取和推送更改的路径。当你运行git fetch命令，Git会从远程仓库（在这种情况下是"origin"）获取最新的提交和分支信息，并将其存储在本地仓库中。当你运行git push命令时，Git会将本地的提交推送到远程仓库（同样是"origin"）</em>
- 完成到这里就可以开始向远程仓库推送和拉取数据了
  
<h3 id ="2">2.fetch & push</h3>

- git push推送数据<br>
  要向远程仓库推送首先要将修改提交到本地仓库，步骤如下：<br>
  (1)在gitbash键入命令<code>git add (改动的文件)  如:git add code.py</code>
  将文件的改动添加到暂存区中<br>
  <i>关于工作区、暂存区和版本库可以参考这篇文章：[Git 工作区、暂存区和版本库](https://www.runoob.com/git/git-workspace-index-repo.html)</i>
  <br>
  (2)接着键入命令<code>git commit -m '关于改动的描述'</code>
  将暂存区提交到版本库
  (3)键入命令<code>git push origin master</code>
  将本地的master分支（init时默认创建的分支）推送到远程仓库<br>
  <i>origin是你想要推送的目的远程仓库主机名（在前面的添加远程仓库步骤中已经将origin和目的github仓库关联起来了），master是要推送的本地仓库分支，如果远程仓库没有该分支就会创建一个新的名为master的分支。</i> 关于git分支可以参考这篇文章：[Git分支管理](https://www.runoob.com/git/git-branch.html)<br>

  <i> <font color=#FF0000>如果在github创建仓库时生成了readme文档则无法直接向该远程仓库推送修改 </font> </i>(会提示：<code>[rejected] master -＞ master (fetch first)error: failed to push some refs.</code>)
 <br>
  这时要先用命令：<code>git pull --rebase origin master</code>将远程仓库的readme拉取到本地仓库。<i>这里的origin和master含义同上</i>
 <br>
<h3 id ="3">3.在github主页面部署密钥和在仓库设置中部署的区别</h3>
<br>

<i>在 GitHub 主页面部署密钥时，该密钥将成为您的全局部署密钥。这意味着该密钥可以用于对您所有的仓库进行操作，包括克隆、推送和拉取等。而在仓库设置中部署密钥时，该密钥将仅限于对该特定库起作用。这种方式更加灵活，因为您可以为每个仓库设置不同密钥，以控制对仓库的访问权限。</i>