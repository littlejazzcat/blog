## 在另一台ubuntu设备上部署ssh并连接原来github账号的远程仓库

- <i>场景：原本以为坏了的电脑重装了ubuntu系统后又能正常使用了，于是想用用这台轻薄本来管理原来github账号的仓库</i>

[1、ubuntu安装git](#1)

[2、部署ssh](#2)

[3、连接远程仓库](#3)

---

<h3 id = 1>1、ubuntu安装git </h3> 

- <code>sudo apt-get install git</code>#使用管理员权限安装git(否则会无法安装)，输入这行命令后会要求输入密码(就是开机时的那个)。提示：ubuntu的终端中输入密码不会回显，也就是说无论你输入了什么，密码后面都不会有显示，在确保自己输入完成后直接按回车就行。
- <code>git --version </code>#查看当前git的版本号


<h3 id = 2>2、部署ssh </h3>

<i>这一步和在windows用gitbash没什么太大区别</i>

1、生成ssh密钥

- <code>git config --global user.name"your name"</code>#设置用户名 
- <code>git config --global user.mail"your email"</code>#设置邮箱
<br>
<i>如果user.name或者user.mail后面什么都不输入就会打印出你之前设置的用户名或邮箱或者使用命令<code>git config --global --list</code>也可以显示用户名和邮箱</i>
<br>
- <code>ssh-keygen -t rsa -C"youremail"</code>#生成密钥信息
- <code>gedit ~/.ssh/id_rsa.pub</code>#复制公钥中的信息（在上一步生成密钥时可以看到你的公钥的位置，就是那个带.pub后缀的，将这个路径放到gedit后回车就可以复制公钥信息了）
  
  <i>这里可能会出现一个问题，就是终端可能会显示找不到文件，我当时是因为不知道ubuntu默认隐藏密码类的文件，解决方法如下：</i>
- <code>cd ~/.ssh</code>来到ssh文件目录下
- <code>ls -a</code>#使用ls -a可以显示所有文件，或者也可在文件中按\<ctr>+h显示所有文件之后就能找到.ssh文件和密钥文件了

<i>不过也有可能是其他问题比如ssh localhost之类的，~~我之前就是不知道文件隐藏了然后去搜了一大堆文章~~ </i>

2、将私钥添加到本地ssh-agent中

- 首先要确保ssh-agent正在运行

  在终端输入命令<code>eval "$(ssh-agent -s)"</code>#直接手动启动ssh-agent

- 将ssh私钥添加到ssh-agent中

  在终端中输入命令<code>ssh-add ~/.ssh/id_rsa</code>#后面那个是私钥的路径（私钥是不带.pub后缀的）

3、将公钥添加到github个人账户中

- 1.复制公钥（这一步前面已经完成了）<br>
   打开前面生成ssh时生成的带.pub后缀名的文件复制里面的内容<br>
- 2.在github中添加密钥<br>
   (1)在任何页面右上角单击自己的资料照片，然后单击设置(settings)<br>
   (2)在边栏的“访问”部分中，单击"SSH和GPG密钥"。<br>
   (3)单击“新建SSH密钥”或“添加SSH密钥”。<br>
   (4)添加title（这只是对这个密钥的描述，方便自己以后识别各个密钥）<br>
   (5)选择密钥类型（长期使用且不经常改动可以选第一个，两种类型具体区别可以自行搜索）。<br>
   (6)在“密钥”字段中，粘贴前面复制的公钥。<br>
   (7)单击“添加SSH密钥”。

4、测试连接

- 在终端输入命令<code>ssh -T git@github.com</code>
- 若成功连接则会显示<code>You've successfully authenticated,</code>

<h3 id = 3>3、连接远程仓库</h3>

- 这一步可以参考这一片文章（具体操作一样）：[添加远程仓库](https://blog.csdn.net/qq_51489920/article/details/131950693?spm=1001.2014.3001.5502)