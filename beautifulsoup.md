# BeautifulSoup库总结


 [ 1、BeautifulSoup库作用 ](#1) <br>
 [ 2、BeautifulSoup()方法 ](#2) <br>
 [ 3、find()、find_all()、selector()、get()方法 ](#3) 
 
 <br>

  
--------

<h3 id = "1">1、BeautifulSoup库作用</h3>
<br>

- 用于将爬取到的网页源码（用requests库完成）解析为soup文档，这样做的好处是可以再用BeautifulSoup库中的过滤函数（方法）过滤提取数据

---

<h3 id = "2">2、BeautifulSoup()方法</h3>

- 基本用法：<br>
  <code>from bs4 import BeautifulSouop
  <br>soup = BeautifulSoup(res.text,'html.parser')
  #这里res是用request的get方法得到的数据，这里暂时省略。
  <br>print(soup.prettify()) </code>

  <br>
  <i>BeautifulSoup方法将res.text（注意要是text格式）解析成标准的html缩进格式数据放入变量'soup'中，这时'soup'是一个soup对象，可以使用BeautifulSoup库的其他方法来提取想要的内容具体看下面的方法。</i><br><br>
  <i>BeautifulSoup支持的解析器如下：<br>

  |解析器名|使用方法|优点|缺点|
  |:----:|:----:|:----:|:----:|
  |Python标准库|BeautifulSoup(markup,"html.parser")|Python内置，速度适中，文档容错能力强|旧版本存在容错能力差的情况|
  |lxml HTML解析器|BeautifulSoup(markup,"lxml")|速度快，文档容错能力强；（推荐）|需要安装C库|
  |Lxml XML解析器|BeautifulSoup(markup,"xml")|速度快，唯一支持XML的解析器|同上
  |html5lib|BeautifulSoup(markup,"html5lib")|容错性最好，以浏览器方式解析文档，生成HTML5格式的文档|速度慢|
</i>
  

---

<h3 id = "3">3、find()、find_all()、selector()、get()方法</h3>
<br>

- find_all()方法
  <br><br>
  <code>
  soup.find_all('div',"item")
  #查找div标签，class = "item" 
  <br>
  soup.find_all('div',class = 'item')
  #结果和前一句一样
  <br>
  soup.find_all('div',attrs = {"class":"item"})
  #attrs参数定义一个字典参数来搜索包含特殊属性的tag
  </code>
  <br><i>
  find_all()方法搜索当前soup对象的所有tag子节点(soup对象就是由各种html节点构成的)并判断是否符合过滤器的条件：<br>
  find_all(name,attrs/class,recursive,string,**kwargs)<br>
  name参数可以查找所有标签名为name的tag
  <br><br></i>

  <font color = 'coral'>

  1、字符串对象自动忽略掉<br>
  例：soup.find_all("title")<br><br>
  1.2、多标签搜索<br>
  例：soup.find_all(name = ['div','p'])<br><br></font>
  <font color = 'yellow'>
  2、基于标签属性查找如：attrs/class_/id/title/...<br><br>
  2.1、通用方式属性搜索——attrs<br>
  soup.find_all(attrs = {'data-custom':'custom'})<br><br>
  2.2、css类名搜索——class_ <font color = 'red'>（注意下划线）</font> <br>
  通过class_参数搜索由指定css类名的tag（可简写）：<br>
  soup.find_all("a","sister")#class_ = "sister"<br><br>
  </font>
  若tag的class属性是多值属性，按照css类名搜索tag时，可以分别搜索tag中的每个css类名：
  <br>例如：<br><code>
  CSS_soup = BeautifulSoup('\<p class = "body strikeout"></p>')
  <br>
  CSS_soup.find_all("p",class_ = "strikeout") #\<p class = "body strikeout"></p>
  <br>
  CSS_soup.find_all("p",class_ = "body") </code>
  <br><br>
  也可以通过CSS值完全匹配来查找
  <br><br>
  <font color = 'skyblue'>
  3、string<br><br>
  3.1、搜索文档中的字符串内容，string参数接受字符串，正则表达式，列表，True。
  <br><br>
  3.2、与其他参数混合过滤<br>
  soup.find_all("a",string = "Else")<br>
  #soup的内容为：[\<a href = "http..." class = "sister">Else</a>]
  <br><br></font>
  <font color = 'pink'>
  4、限定直接子节点——recursive<br>
  加入参数recursive = False 则会只搜索tag的直接子节点
  </font><br><br>

- selector方法<br><br>

  <font color = 'Aquamarine'>

  soup.select('div.item > a > h1')
  #要加单引号，括号内容可以通过浏览器复制得到（鼠标右键目标内容然后点击copy selector
  <br>
  <font color = 'red'>！！！li:nth-child(1)需要改为li:nth-of-type(1)
  <br><br></font>
  需要注意这种方法得到的结果是会带有标签的，使用get_text()方法即可获得其中的文本内容<br>
  即在使用完select后再soup2 = soup.get_text().strip()<br><br>
  <font color = 'violet'>
  补充：<br>
  strip()函数是用于移除字符串开头和末尾的指定字符（默认为空格）或字符序列的方法。下面是使用strip()函数的基本语法：<br><br>
  string.strip([characters])<br>
  其中，string是要操作的字符串，characters是可选参数，用于指定要移除的字符或字符序列。</font></font>

  下面是一些示例：

  移除字符串开头和末尾的空格：<br>
  <code>
  s = "   Hello, World!   "<br>
  print(s.strip())  # 输出: "Hello, World!"<br></code>
  移除字符串开头和末尾的指定字符：
  <code>
  s = "***Hello, World!***"<br>
  print(s.strip("*"))  # 输出: "Hello, World!"<br></code>
  移除字符串开头和末尾的多个指定字符序列：
  <code>
  s = "\~~Hello,World~~~~" <br>
  print(s.strip("~"))  # 输出: "Hello, World!" <br></code>
  需要注意的是，strip()函数返回一个新的字符串，并不会修改原始字符串。如果想要移除字符串中间的字符，可以考虑使用其他字符串处理方法，比如replace()函数或正则表达式等。
  </font></font>
  <br><br>
  </i>

- get()方法
  <br><br>
  <font color = 'Khaki'>
  <i>
  BeautifulSoup解析得到的soup对象有一个get()方法。该方法用于获取标签（HTML元素）的属性值。
  使用get()方法时，你需要传递属性名作为参数。如果该属性存在，get()方法将返回对应的属性值；如果属性不存在，get()方法可以返回默认值（可选参数）或者None。</i>
  </font><br><br>
  <font color = 'red'>
  注意区分字典、requests库和beautifulsoup库中的get方法，它们的作用分别是取键值、向网址请求获取网页资源、获取属性值。






