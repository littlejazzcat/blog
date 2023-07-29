# requests库


 [ 1、requests库作用 ](#1) <br>
 [ 2、get方法 ](#2) <br>
 [ 3、post方法 ](#3) <br>
 [ 4、requests库常见抛出异常 ](#4)
  
--------

<h3 id = "1">1、requests库作用 </h3> 
<br><br>

- requests库是一个用于发送HTTP请求的Python库。它提供了一种简单而直观的方式来与Web服务进行交互，例如在爬取网页数据、访问API或进行HTTP通信等方面。使用requests库，你可以轻松地向服务器发送GET、POST、PUT、DELETE等各种类型的请求，并获取响应结果。
- 主要功能包括：

  1、发送HTTP请求：使用requests库，你可以发送各种类型的HTTP请求，如GET、POST、PUT、DELETE等，以及定制请求头、请求参数、文件上传等功能。

  2、处理响应结果：requests库能够处理服务器返回的响应结果，包括获取响应状态码、响应头、响应正文等信息。你可以根据需要对响应结果进行解析和处理。

  3、Session管理：requests库支持使用Session对象来管理会话，可以在多个请求之间保持会话状态，例如保存登录信息、设置Cookie等。

  4、身份认证：requests库提供了基本身份认证、摘要认证和OAuth等身份认证机制，可以轻松地在请求中添加身份凭证。

  5、SSL验证：requests库默认会验证HTTPS请求的SSL证书，你可以自定义验证方法或禁用SSL验证。

  6、文件上传和下载：requests库可以方便地处理文件的上传和下载，支持多种文件传输方式，如普通文件上传、分块上传、断点续传等。<br>

--------

<h3 id = "2">2、get方法 </h3>
<br><br>

- 基本使用方法：<br>
  <code>import requests
  <br>headers = {'User-Agent':'xxx'(用于伪装成浏览器)}
  <br>res = requests.get('url',headers = headers)
  #get方法获取网址中的内容并保存在res中
  <br>print(res.text)
  </code><br><br>
  <i>GET 方法返回的数据类型没有固定规定，而是根据服务器端实际配置和所请求资源的内容来确定。通常，通过查看响应头部的 Content-Type 字段可以了解服务器返回的数据类型。<br>
  通常有以下类型的数据：<br>
  1、文本数据：服务器可以返回纯文本数据，如普通文本、HTML、XML、JSON 等。这些数据类型通常以字符串的形式返回。

  2、图像数据：当请求的资源是图像文件时，服务器可以返回图像数据，如JPEG、PNG、GIF 等格式。这些数据类型以二进制形式返回。

  3、二进制数据：有时，服务器可能会返回二进制数据，例如文件下载或媒体文件。这些数据以字节流的形式返回。

  4、文件数据：在某些情况下，服务器可能直接返回文件数据，并提供下载链接。这样的响应可能包含文件的元数据和字节流。</i>

----------

<h3 id = "3">3、post方法 </h3>
<br><br>

- 基本使用方法：<br>
  <code>import requests
  <br>url = "https://example.com/api"
  <br>data = {"key1": "value1", "key2": "value2"}
  #创建的字典即为要提交的数据
  <br>headers = {"Content-Type": "application/json"}
  <br>response = requests.post(url, json=data, headers=headers)
  #获取的响应存在response中
  </code><br><br>
  <i>该例子使用post()方法发送请求，并将响应存储在response变量中。该方法接受两个参数：URL和数据。你可以通过传递一个字典或一个字符串作为数据来发送POST请求。
  除了data参数，post()方法还有其他一些可选参数，如headers、params、json等。这些参数可以用于设置请求头、查询字符串参数或发送JSON数据。</i>
  <br><br>

-----

<h3 id = "4">4、requests库常见抛出异常 </h3>
<br><br>

- ConnectionError ：网络问题，如DNS查询失败，拒绝连接...
- Response.raise_for_status()抛出一个HTTPError异常：404错误等
- Requests抛出一个Timeout异常；请求超时
- Requests抛出一个TooManayRedirects异常：请求超过设定的最大重定向次数
  <br><br><i>
  为避免这些异常导致整个程序停止而需全部重新运行的情况，可以使用python的try-except语句，示例如下：<br>
  <code>import requests
  <br>try:
  <br>response = requests.get('https://example.com', timeout=5)
  <br># 处理响应数据
  <br>print(response.text)
  <br>except requests.exceptions.Timeout:
  <br># 请求超时异常处理
  <br>print("请求超时，请稍后重试或检查网络连接。")
  <br>except requests.exceptions.RequestException as e:
  <br># 其他请求异常处理
  <br>print("请求发生错误:", e)
  </code>
<br><br><i>
通过将请求代码放置在try块中，可以捕获可能抛出的Timeout异常。然后，可以根据具体情况进行适当的处理，例如打印错误信息、重试请求或执行其他操作（即当出现对应异常时就会跳过try:下的语句而去执行except:下的语句）</i>


 



