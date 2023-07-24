# **github ssh部署记录**

- [1.获取repository的ssh](#1)
- [2.将ssh密钥添加到ssh-agent](#2)
- [3.将公钥添加到github个人账户中](#3)
- [4.测试连接](#4)

---

<h3 id ="1">1.通过gitbash获取公私钥</h3>


- 在gitbash终端输入[ssh-keygen -t ed25519 -C "your_email@example.com" ]()<br>

- 终端则会显示：
[Enter file in which to save the key (C:\Users\13488/.ssh/id_ed25519):]()<br>
*（后面那个路径以自己的为准）若冒号后不填写路径直接回车则会将公私钥生成在默认位置即冒号左边显示的路径。.pub后缀的为公钥另一个是私钥。*

<h3 id ="2">2.将公钥添加到github个人账户中</h3>

- 1.确保ssh-agent正在运行<br>
   在gitbash键入[eval "$(ssh-agent -s)"]()以手动启动
   若启动失败（出现error:1058）则以管理员身份打开powershell后在终端键入
   [Set-Service -Name ssh-agent -StartupType automatic]()
- 2.检查是否已将ssh密钥添加到ssh-agent<br>
   在gitbash键入[ssh-add -l]()  
   若显示The agent has no identities.说明还未添加。
- 3.将ssh私钥添加到ssh-agent<br>
   在gitbash终端键入[ssh-add ~/.ssh/id_ed25519]()<br>
   *若前面生成ssh密钥时没有使用默认位置的则在ssh-add 后面填入私钥的位置（注意！是不含.pub后缀的那个文件）*


<h3 id ="3">3.将公钥添加到github个人账户中</h3>

- 1.复制公钥<br>
   打开前面生成ssh时生成的带.pub后缀名的文件复制里面的内容
- 2.在github中添加密钥<br>
   (1)在任何页面右上角单击自己的资料照片，然后单击设置(settings)<br>
   (2)在边栏的“访问”部分中，单击"SSH和GPG密钥"。<br>
   (3)单击“新建SSH密钥”或“添加SSH密钥”。<br>
   (4)添加title（这只是对这个密钥的描述，方便自己以后识别各个密钥）<br>
   (5)选择密钥类型。<br>
   (6)在“密钥”字段中，粘贴前面复制的公钥。<br>
   (7)单击“添加SSH密钥”。

<h3 id ="4">4.测试链接</h3>

- 在gitbash终端键入[ssh -T git@github.com]() <br>
- 若成功连接则会显示[ You've successfully authenticated,]()
- 对于公钥和私钥的详细内容可以参考这篇文章：[图解SSH原理](https://www.cnblogs.com/diffx/p/9553587.html)
  


