# re正则表达式

[1、预定义字符](#1)<br>
[2、数量词](#2)<br>
[3、边界匹配](#3)<br>
[4、修饰符](#4)<br>
[5、search()、sub()、findall()方法](#5)<br>

---

- 0、一般字符
   <br><br>
  |字符|意义|
  |:---:|:---:|
  |.|a.c → abc,aic,a&c等（不包括换行符|
  |\ |转义字符，让字符变回原来的意思|
  |[...]|在括号中任选一个：a[bcd] → ab,ac,ad|
---
  <br>

- 1、预定义字符<h4 id = '1'></h4>

  |字符|意义|
  |:---:|:---:|
  |\d|匹配一个数字字符，等价于[0-9]|
  |\D|匹配一个非数字字符，等价于[^0-9]|
  |\s|匹配任何空白字符如空格、制表符、换页符等等价于[\f,\n,\t,\r,\v]|
  |\S|非空白字符等价于[^\f,\n,\t,\r,\v]|
  |\w|匹配任意字母（包括下划线）等价于[A-Z,a-z,0-9,_]|
  |\W|匹配非单词字符为\w取反|
  <br>
---

- 2、数量词<h4 id = '2'></h4>
  
  |字符|意义|
  |:---:|:---:|
  |*|匹配前一个字符0或无限次，如ab\*c → abbc,ac,abc等|
  |+|匹配前一个字符1或无限次，如ab+c → abc,abbc,abbbc等|
  |？|匹配前一个字符0或1次，如ab?c → ac,abc,|
  |{m}|匹配前一个字符m次|
  |{m,n}|匹配前一个字符m到n次|
  <br>

- 3、边界匹配（了解，爬虫中很少用）<h4 id = '3'></h4>
  
  |字符|意义|
  |:---:|:---:|
  |^|匹配字符串的开头，如^abc → 匹配以abc开头的字符串|
  |$|匹配字符串的结尾，如abc$ → 匹配以abc结尾的字符串|
  |\A||
  |\Z||
<br>
<font color = 'yellow'>

- (\.\*?) : "()"中的内容作为返回结果，",*?"匹配任意字符</font>

  <br>
---

- 4、修饰符<h4 id = '4'></h4>
  
  <i>修饰符主要配合后面的各种方法使用</i>
  |字符|意义|
  |:---:|:---:|
  |re.I|使匹配对大小写不敏感|
  |re.L|做本地化识别匹配|
  |re.M|多行匹配，影响^和$|
  |<font color = 'red'>re.S|使匹配包括换行符在内的所有字符（做多行匹配时用）|
  |re.U|根据Unicode字符集解析字符。影响\w,\W,\b,\B|
  |re.X|给予更灵活的格式，以便将正则表达式写得更易理解|

  <br>
  <font color = 'Aquamarine'>
  <i>使用的最多的是re.S ，当要匹配的内容有多行时可以使用这个修饰符.<br>
  例如：findall()方法是逐行匹配的，当某行没有匹配到数据时就会从下行开始，这时可以用re.S修饰符使它不再从头开始匹配。<br>
  <code>
  import re<br>
  a = '''<某标签>数据<br>
  <某标签>'''<br>
  word = re.findall('<某标签>(.*?)<某标签>',a)<br>
  print(word)<br></code>
  结果得到一个空列表，若加上re.S修饰符即<code> word = re.findall('<某标签>(.*?)<某标签>',a,re.S)</code> 则会打印"数据 "。</i></font>
  <br>
  <br>
---

- 5、search()、sub()、findall()方法<h4 id = '5'></h4>
  
    <h4>findall(pattern,string,flag = 0)<br></h4>
      - para:<br>
      - (1)pattern : 要匹配的正则表达式<br>
      - (2)string : 要匹配的字符串<br>
      - (3)flags : 标志位（即修饰符），用于控制正则表达式的匹配方式，比如换行匹配，是否区分大小写等。<br>
    <br><br>

    <h4>search(pattern,string,flags = 0)<br></h4>
      - para:<br>
      - (1)pattern : 要匹配的正则表达式<br>
      - (2)string : 要匹配的字符串<br>
      - (3)flags : 标志位（即修饰符），用于控制正则表达式的匹配方式，比如换行匹配，是否区分大小写等。<br><br>
    <i>search方法用于在给定的字符串中搜索与指定的模式匹配的内容，并返回一个匹配对象（Match object）。匹配对象是包含匹配结果的特殊对象，它具有各种属性和方法，可用于获取有关匹配的详细信息。如果找到了匹配项，则search方法返回匹配对象；如果没有找到匹配项，则返回None。</i>
    <br>例：<br><code>
    import re<br>
    a = 'one1two2three3'<br>
    infos = re.search('\d+',a)<br>
    print(infos)<br>
    </code>
    <br>
    结果：<_sre.SRE_Match object; span(3,4),match='1'><br>
    <code>print(infos.group())</code><font color = 'violet'>#可以通过group方法获取匹配对象的值。在这种情况下，match.group()返回整个匹配项，match.group(1)返回第一个捕获组，match.group(2)返回第二个捕获组</font><br></code>
    <font color = 'red'><i>search方法只返回第一个匹配项，因此它通常用于判断是否存在匹配项或仅需获取第一个匹配项的情况。而findall方法适用于需要获取所有匹配项的情况，且findall方法返回的是一个列表。</i></font><br>
    <br><br>
    <h4>sub(pattern,repl,string,count = 0,flags = 0)<br></h4>
      - para:<br>
      - (1)pattern: 要匹配的正则表达式<br>
      - (2)repl: 要用来替换的字符串<br>
      - (3)string: 要被查找替换的原始字符串<br>
      - (4)count: 模式匹配后替换的最大次数，默认0表示替换所有的匹配<br>
      - (5)flags: 标志位，同search<br>
    <br>
    例：将123-456-789中的"-"去掉<br>
    <code>
    import re<br>
    phone = '123-456-789'<br>
    new_phone = re.sub('\D','',phone)<br>
    print(new_phone)<br></code>
    <i>结果为：123456789


