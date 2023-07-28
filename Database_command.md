# MongoDB & MySQL指令大全
<h4>
<i>
主要用于总结爬取信息时使用到的以及学习到的指令
</i>
</h4>

--------------------

 1、[MongoDB指令](#1)

 2、[MySQL指令](#2)


--------------------

<h3 id ="1">1、MongoDB</h3>

- 命令行指令：<br><br>
  在安装路径的bin文件夹下打开命令行窗口后输入:
  <code>mongo</code>
  <br><i>
  若已配置好了环境(将bin的路径加到环境变量里)则可在任意位置使用该指令</i>
  <br><br>
  <code>show dbs</code>  查看数据库
  <br><br>
  <code>use xxx</code>（数据库名称） 打开某数据库<br><i>若不存在则会创建</i>
  <br><br>
  <code>show collections</code>  查看当前数据库中的集合（类似于数据表格）
  <br><br>


- Pymongo库命令
  <br><br>
  <code>import pymongo #导入库
  <br>
  client = pymongo.MongoClient('localhost',271017) #连接数据库（27017为默认端口号）
  <br>
  mydb = client['mydb'] #新建mydb数据库（类似于一个excel文件）<br>
  test = mydb['test'] #新建test数据集合（类似于excel文件中的一张表格）
  <i><br>
  </code>
  实测：若该数据库或数据集合已存在则不会再重新创建，后续数据insert则会加在已有数据后面（记得在需要重新导入数据时要先清除前面的数据）<br><code></i>
  test.insert_one({'name':'Jan','sex':'男','grade':89}) #插入数据<br>
  </code>




<br>

--------------------

<h3 id ="2">2、MySQL</h3>
  
- 命令行指令：<br><br>
  在安装路径的bin文件夹下打开命令行窗口后输入:
  <code>mysql -uroot -p(你的密码)</code>
  <br><i>
  若已配置好了环境(将bin的路径加到环境变量里)则可在任意位置使用该指令</i>
  <br><br>
  <code>show databases</code>  查看数据库
  <br><br>
  <code>use xxx</code>（数据库名称） 打开某数据库<br><i>若不存在则会创建</i>
  <br><br>
  <code>show tables</code>  查看当前数据库中的集合（类似于数据表格）
  <br><br>
  <code>create database mydb;</code> 建立数据库<br><br>
  <code>CREATE TABLE students (<br>
    name char(5),<br>
    sex char(1),<br>
    grade int<br>
    )ENGINE INNODB DEFAULT CHARSET=utf8 ;</code>创建数据表<br><br>
  <code>insert into students(name,sex,grade) values ("小明","男",92);</code>插入数据<br>
- Pymysql库命令
  <br>
  <code>
  import pymysql
  conn = pymysql.connect(host='localhost', user='root', <br>passwd='(你设置的密码)',db='mydb',port=3306,charset='utf8')
  #连接数据库
  cursor = conn.cursor() #光标对象
  cursor.execute("insert into students(name,sex,grade) values(%s,%s,%s)", ('Peter','woman',87)) #插入数据
  conn.commit()

  
