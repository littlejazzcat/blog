# Scrapy框架学习记录


 [ 1、Scrapy框架基本使用方法 ](#1) <br>
 [ 2、Scrapy框架是什么？有什么作用？ ](#2) <br>
 [ 3、Scrapy框架的优缺点 ](#3) 
 
 <br>

  
--------


<h3 id = "1">Scrapy框架基本使用方法</h3>
<br>

- 安装scrapy
  
  使用命令<code>pip install scrapy</code>进行安装即可。<br>
  [scrapy官网](https://scrapy.org/) <br>
  [scrapy文档](https://doc.scrapy.org/en/latest/intro/tutorial.html) <br>
  [scrapy更新日志](https://docs.scrapy.org/en/latest/news.html)

- 创建scrapy项目
  
'''

    #先通过命令行进入想要创建scrapy项目的位置再键入如下命令(cmd命令行)
    scrapy startproject <项目名>

    #或者使用如下命令可以直接在想要的位置创建项目
    scrapy startproject <项目名> <dir>

    #例：scrapy startproject myspider d:/study

    这一步之后在目标位置下已经创建了一个scrapy项目文件夹，其结构如下：(这时spiders文件夹下还只有一个__init__.py文件)
<br>

![](https://userblink.csdnimg.cn/defc122894d24b53b7c14d2395e5b673.png)

<br>


- 生成一个爬虫文件

'''

    #先进入刚刚创建的爬虫项目文件夹中
    cd myspider  #刚刚创建项目时的项目名称

    #接着键入命令如下以创建一个爬虫文件（这些文件的具体作用放在后面）
    scrapy genspider [-t template] <name> <domain>

    例：scrapy genspider jzspider bilibili.com

    # name是爬虫名字（这个不是前面那个项目名字），domain用在爬虫文件中的alowed_domains和start_urls数据中分别是允许访问域名和起始url，[-t template]表示可以选择生成的文件模版（不加这项则默认为basic模版）（暂时先用着basic模版要查看有哪些模版可以键入<code>scrapy genspider -l</code>

    #在这步之后会在myspider/spiders位置下生成一个jzspider.py文件其中内容如下
    
    import scrapy

    class JzspiderSpider(scrapy.Spider):
        name = "jzspider"
        allowed_domains = ["bilibili.com"]
        start_urls = ["https://bilibili.com"]

        def parse(self, response):
            pass

    #其中几个属性就是前面键入的命令中的,其中的parse方法就是用于接受下载中间件（这个后面会提到）传过来的response并定义对网站的相关操作。
    #演示例子如下（爬取b站的标题栏）：

    import scrapy
    from ..items import MyfirstspiderItem

    class JzspiderSpider(scrapy.Spider):
        name = "jzspider"
        allowed_domains = ["bilibili.com"]
        start_urls = ["https://www.bilibili.com"]

        def parse(self, response):
            # 获取所有的专栏名称节点
            column_list = response.xpath('//div[@class = "channel-items__left"]/a[@class = "channel-link"]/text()')
            print(f'column_list type:{type(column_list)}\n')
            #print(column_list)
            #遍历专栏节点列表
            i = 0

            for column in column_list:
                # xpath方法返回的是选择器对象列表   extract()方法可以提取到selector对象中data对应的数据。
                print(column)
                column_name = column.extract()
                print(f'column{i+1}:{column_name}')
                i = i+1
                item = MyfirstspiderItem()

                item['title'] = column_name
                #yield将数据传送到管道中
                yield item
                # 爬虫文件中提取数据的方法每yield一次，就会运行一次

            pass

- 数据建模
  
  以防手误抓错数据字段，接着要再进行数据建模即在items文件中定义需要抓取的字段，运行过程中，系统会自动检查，值不相同会报错。<br>
  示例如下：

'''

    import scrapy


    class MyfirstspiderItem(scrapy.Item):
        # define the fields for your item here like:
        # name = scrapy.Field()
        # 标题
        title = scrapy.Field()

- 利用数据管道保存数据
  
'''

    import json

    class MyfirstspiderPipeline(object):

        def __init__(self):
            self.file = open('myfirstspider.json', 'w', encoding='utf-8')

        def process_item(self, item, spider):

            # 该方法为固定名称函数
            item = dict(item) #将item对象强制转换成字典（scrapy中特有方法）
            '''ensure_ascii=False'''
            json_data = json.dumps(item, ensure_ascii = False, indent = 2) + ',\n'

            #将数据写入文件
            self.file.write(json_data)

            print('Myfirstspider:',item)
            # 使用完管道，需要将数据返回给引擎
            return item
        
        def __del__(self):
            self.file.close()

- 配置启动管道
  
  找到settings文件（具体需要的代码已经在生成文件的时候就以注释形式写好了）解封需要的代码（去掉"#"）<br>
  具体如下：

  管道配置代码解封：
  ![](https://userblink.csdnimg.cn/39013e88f4ff45499bfa5e35aae564c2.png)

  设置user-agent:
  ![](https://userblink.csdnimg.cn/f127fdfba78f4f7e82aecc44b37ec698.png)

  <i>在Accept-Language下加上"User-Agent"</i>

- 启动爬虫
  
  从cmd进入spiders所在位置后键入如下命令：<code>scrapy crawl jzspider</code>（crawl后面的名称就是前面创建爬虫文件时用的名字

  示例代码最终会在spiders目录下生成一个json文件内容如下：
  ![](https://userblink.csdnimg.cn/ac7677eccbe54fffb119bc1f78e6d2e5.png)

  <br>
<h3 id = "2">2、Scrapy框架是什么？有什么作用？</h3>
<br>

- Scrapy是什么
  
  Scrapy是一个用于Python的开源Web抓取框架，它提供了一种简单而强大的方式来抓取和处理网络数据。Scrapy框架由多个组件组成，每个组件在整个抓取过程中起着不同的作用。<br>

- Scrapy的各个组件

    - Scrapy Engine（引擎）: 引擎是整个框架的核心，负责控制整个抓取过程的流程和数据流。它接收用户的操作指令，并将指令传递给各个组件执行。

    - Scheduler（调度器）: 调度器是负责接收引擎发送的请求，并根据一定的策略决定如何将其发送给下载器。它维护了一个待抓取的队列，并负责处理重复操作、优先级等问题。

    - Downloader（下载器）: 下载器负责将引擎发送的请求发送给指定的URL，并下载响应数据。它可以处理各种类型的请求，并且支持异步、并发的下载操作。

    - Spider（爬虫）: 爬虫是用户自定义的类，继承自Scrapy提供的基类，用于定义如何从下载的页面中提取数据。爬虫解析响应，并根据用户设定的规则提取所需的数据，并在需要时生成新的请求。

    - Item Pipeline（管道）: 管道负责处理从爬虫中提取的数据。它是一个可选的组件，用于清洗、验证和存储数据。可以将数据保存到数据库、文件或其他数据存储系统中。

    - Downloader Middleware（下载器中间件）: 下载器中间件是介于下载器和引擎之间的钩子函数，可以在请求发送给下载器前或响应返回给引擎前对请求和响应进行修改或处理。

    - Spider Middleware（爬虫中间件）: 爬虫中间件是介于引擎和爬虫之间的钩子函数，可以对请求和响应进行处理，例如添加或删除请求头、处理代理、处理异常等。
  
  <br><i>其中Spider(爬虫)、Item Pipeline(管道)是需要用户自己编写的，Spider Middleware（爬虫中间件)、Downloader Middleware(下载器中间件)的运行的逻辑位置不同但是作用相同，用户可以自己更改用于替换UA或者更改代理等。</i>

- 各组件运行逻辑：
  <br>
  引擎是整个Scrapy框架的核心，负责控制各个组件之间的协调和通信。1、它从调度器中取出一个Request，将其发给下载器进行处理。2、下载器负责下载引擎发送的Request，并将下载的结果封装成Response返回给引擎。(下载器可以处理各种类型的请求，如HTTP、HTTPS等，并进行页面内容的下载与处理。在请求的过程中，下载器还可以通过中间件进行一些额外的操作，如添加Proxy、处理Cookies。)3、引擎将下载器返回的Response交给Spider进行解析。4、Spider是用户定义的用于解析网页页面并提取数据的组件，Spider处理完下载器返回的Response后，将提取的数据返回给引擎。5、引擎再将数据传递给管道进行持久化或其他处理。在这个过程中，中间件是位于引擎与下载器之间的组件，可以在请求下载和响应处理的过程中进行干预和扩展。中间件可以对请求和响应进行修改、过滤、错误处理等操作。常用的中间件包括User-Agent中间件、Proxy中间件、Cookie中间件等。
  <br>

<h3 id = "1">3、Scrapy框架的优缺点</h3>
<br>

- Scrapy的优点
  
    - 高效性：Scrapy使用异步处理和并发机制，能够快速高效地抓取网页数据。
    - 可定制性：Scrapy提供了丰富的可定制的组件和中间件，可以根据需求对爬虫进行个性化的配置和扩展。
    - 扩展性：Scrapy支持多种储存方式（如MySQL、MongoDB等），可以方便地将爬取的数据存储到各种数据库或文件系统中。
    - 自动化处理：Scrapy提供了自动化的处理机制，包括自动解析页面、跟踪链接、处理表单等，可以大大减少开发者的工作量。
    - 文档丰富：Scrapy拥有完善的官方文档以及广泛的社区和贡献者支持，能够提供详尽的使用教程和示例。

<br>

- Scrapy的缺点

    - 学习曲线较陡峭：相对于编写简单的爬虫脚本，Scrapy框架需要学习更多的概念和使用方法。

    - 配置复杂：Scrapy需要针对每个爬虫项目进行相应的配置，包括定义项目结构、编写爬虫规则、设置中间件等。

    - 依赖于框架：Scrapy框架自身存在一些依赖，对于一些简单的爬虫任务而言，使用框架可能显得过于笨重不必要。

    - 面向复杂场景：Scrapy框架更适用于处理复杂的爬虫任务，如果只是进行简单的网页抓取，使用框架可能有些繁琐。

- 总的来说，Scrapy框架相对于普通爬虫脚本在处理复杂场景时具有更大的优势，但在简单的爬虫任务上可能会显得不太合适。
            



