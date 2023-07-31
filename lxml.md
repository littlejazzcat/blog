# lxml库及xpath总结

[1、Lxml库简介及作用](#1)<br>
[2、HTML方法、tostring方法](#2)<br>
[3、xpath语法](#3)<br>


---

<h3 id = "1">1、Lxml库简介及作用</h3>
<br>

- Lxml库是基于libxml2的XML解析库的封装。只用C语言编写，用xpath语法解析定位网页数据<br>

- 导入方法：<code>from lxml import etree</code>
<br>
---

<h3 id = "2">2、HTML方法、tostring方法</h3>
<br>

- lxml库中的HTML方法将文档解析成一个Element对象，它是lxml库中的核心数据结构之一。Element对象表示XML或HTML文档中的一个元素，并以树形结构保存了整个文档的层次关系和内容。
Element对象具有类似于字典的属性和方法，可以通过标签名、属性等方式访问和操作文档中的元素和数据。<br><br>
使用方法如下：<br>
<code>
from lxml import etree<br>
#从字符串解析HTML<br>
html_string = \"\<html>\<body>\<h1>Hello, World!\</h1>\</body>\</html>" <br>
html_tree = etree.HTML(html_string)<br>
<br></code>
<font color = 'Aquamarine'>
HTML方法同时还具有自动修正HTML代码的功能，<br>
比如将上面的<code>html_string= \"\<html>\<body>\<h1>Hello, World!\</h1>\</body>\</html>"
</code> <br>
改为 <code> html_string = \"\<html>\<body>\<h1>Hello, World!\</h1>\</body>" </code> 最终得到的结果是一样的，结尾的\</html>会被自动补上。<br><br></font>
<i><font color = 'red'>
具体的属性和方法等后面学到或者用到了记得补上。
</i></font>
<br>



---
<h3 id = "3">3、xpath语法</h3>
<br>

- 节点选择
  
  |字符|意义|
  |:---:|:---:|
  |node|选择此节点的所有子节点|
  |/|从根节点开始选择|
  |//|从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。|
  |.|选取当前节点|
  |..|选取当前节点的父节点|
  |@|选取属性|

- 节点选择实例

  |字符|意义|
  |:---:|:---:|
  |user_database|选取元素user_database所有子节点,例如div，h2等|
  |/user_database|选取根元素user_database|
  |user_database/user|选取属于user_database的子元素的所有user元素|
  |//user|选取所有user子元素，不管它们在文档的位置|
  |user_database//user|选择属于user_database元素后代所有user元素，不管它们位于user_database下的什么位置|
  |//@attribute|选取名为attribute的所有属性|

- 谓语
  
  |字符|意义|
  |:---:|:---:|
  |/user_database[1]|选取user_database子元素的第一个user元素|
  |//li[@attribute]|选取所有拥有名为attribute属性的li元素|
  |//li[@attribute = 'red']|选取所有li元素且这些元素都有值为red的属性|

- 通配符及逻辑运算符选择
  
  |字符|意义|
  |:---:|:---:|
  |*|匹配任意节点|
  |@*|匹配任意属性节点|
  |and|与操作符，同时满足两个条件|
  |or|或操作符，满足任意一个条件|
  |not|非操作符，不满足指定条件的节点|
<br>

- 在最后加上/text()可以获取标签内文本信息如：<br>
  <code>
  id = selector.xpath('//*[@id = "qiushi_tag"]/div/a[2]/h2/text()')
