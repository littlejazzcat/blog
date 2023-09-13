# Selenium学习记录


 [ 1、Selenium作用 ](#1) <br>
 [ 2、driver模块 ](#2) <br>
 [ 3、selenium执行JS代码 ](#3) <br>
 [ 4、ActionChains](#4) <br>
 [ 5、等待模块](#5)<br>
 
 
--------

<h3 id = 1>1、Selenium的作用</h3>
Selenium 是一个用于自动化浏览器操作的工具，它可以模拟用户在浏览器中的行为，进行网页测试、爬虫开发等各种任务。

<h3 id = 2>2、常用命令</h3>

- driver模块

```python

    from selenium import webdriver
    from selenium.webdriver.common.by import By
    
    #确认web驱动，常用的有
    #webdriver.Chrome()
    #webdriver.Edge()
    #webdriver.Firefox()
    opt = webdriver.ChromeOption() #创建chrom参数对象
    opt.headless = True #设置无头浏览器才需要这个opt
    driver = webdriver.Chrome()#可加入参数option=opt 以使用无头浏览器对象（需要该浏览器支持才可以）
    #设置浏览器大小
    driver.set_window_size(800,600)
    #最大化浏览器
    driver.maximize_window()
    
    driver.get("https://www.csdn.net/") #驱动浏览器访问网址
    driver.quit() #回退
    driver.forward() #前进
    driver.refresh() #页面刷新

####窗口跳转
    all_handles = driver.window_handles  # 获取所有窗口句柄
    print(all_handles)
    driver.switch_to.window(all_handles[0])  # 切换到首页句柄
    driver.switch_to.parent_frame()  #跳转到父级窗口
    driver.switch_to.default_content() #跳转到默认页

    alt = driver.switch_to.alert  #捕获网页弹窗


    #查找页面中的元素并输入关键字参数（多为向搜索框输入关键字）
    driver.find_element(by=By.ID, value='toolbar-search-input').send_keys('python')
    
    #查找页面中对应的元素并实现点击操作(click())
    driver.find_element(by=By.ID, value='toolbar-search-button').click()
 
    
####常用元素查找方法(这里还用了By模块)：

    driver.find_element(By.XPATH, '//*[@id="kw"]') 
    # 根据xpath选择元素(万金油)
    driver.find_element(By.CSS_SELECTOR, '#kw') 
    # 根据css选择器选择元素
    driver.find_element(By.NAME, 'wd') 
    # 根据name属性值选择元素
    driver.find_element(By.CLASS_NAME, 's_ipt') 
    # 根据类名选择元素
    driver.find_element(By.LINK_TEXT, 'hao123') 
    # 根据链接文本选择元素
    driver.find_element(By.PARTIAL_LINK_TEXT, 'hao') 
    # 根据包含文本选择
    driver.find_element(By.TAG_NAME, 'title') 
    # 根据标签名选择
    # 目标元素在当前html中是唯一标签或众多标签第一个时候使用
    driver.find_element(By.ID, 'su') 
    # 根据id选择

    title = driver.title #获取页面的标题

    #浏览器页面截屏并保存到参数对应的地址
    driver.save_screenshot("./jianshu.png")

    #关闭浏览器
    driver.quit()
    #关闭浏览器当前标签
    driver.close()

####设置无头浏览器的另一种方法
    from selenium import webdriver
    from selenium.webdriver.chrome.options import Options

    opt = Options()  # 创建 chrome 参数对象
    opt.add_argument('-headless')  # 无头参数

    driver = webdriver.Chrome(options=opt)  # 创建 chrome 无界面对象

    driver.get("http://www.jianshu.com")  # 打开简书

    title = driver.title
    print("网页标题是：", title)
    driver.quit()

```
- selenium执行JS代码(主要通过excute_script("js代码")方法实现)
<h3 id = 3></h3>

```python
    from selenium import webdriver
    from selenium.webdriver.chrome.options import Options

    opt = Options()  # 创建 chrome 参数对象

    driver = webdriver.Chrome()

    driver.get("http://www.jianshu.com")  # 打开简书

    js1 = "$('title').text()"
    ret1 = driver.execute_script(js1) # 返回 None


    js2 = "return $('title').text()"
    ret2 = driver.execute_script(js2) # 返回获取到的标题

    print(ret1,ret2)
```

- ActionChains <br>
<i><h6 id = 4>动作链主要用于模拟鼠标操作，例如单击、双击、右键、拖拽、操作键盘（按按键、松开按键、输入等）</h6></i>

```python
###调用ActionChains的方法后，它不会立刻执行，而是将所有操作放到一个队列中，直到调用perform()方法后，队列依次执行

###关于鼠标的方法：
    click(on_element=None) #普通点击
    click_and_hold(on_element=None) #按住左键不松开
    context_click(on_element=None) #右击
    double_click(on_element=None) #双击
    drag_and_drop(source,target) #拖动某个元素到目标元素
    drag_and_drop_by_offset(source,xoffset,yoffset) #拖拽某个元素到某个坐标
    move_by_offset(xoffset,yoffset)#鼠标移动到某个位置
    move_to_element(to_element) #移动鼠标到某个元素
    move_to_element_with_offset(to_element,xoffset,yoffset)#移动鼠标到距离某个元素（左上角坐标）指定的位置
    release(on_element=None) #释放鼠标左键

###关于键盘
    send_keys(*keys_to_send) #发送内容到当前焦点的元素（就是要先找到元素再调用这个方法）
    send_keys_to_element(element,*keys_to_send) #发送内容到指定元素
    key_down(value,element=None) #按下某个键盘上的键
    key_up(value,element=None) #松开某个键

```
- 等待模块(隐式等待与显式等待)
<h6 id = 5><i>显式等待（Explicit Wait）是一种明确指定等待条件的方法，它会在代码中手动设置等待时间，并且等待直到满足指定条件。这种等待方法需要明确定义等待的条件，例如等待页面中的某个元素可见或可点击，以及设置等待的时间限制。

隐式等待（Implicit Wait）是一种全局性的设置，适用于整个WebDriver实例的生命周期。在使用隐式等待后，WebDriver会在试图查找或操作元素时等待一段时间，直到元素出现或超过指定的等待时间为止。隐式等待是一种全局性的等待设置，不需要每次等待都明确指定等待条件，它会在整个代码执行过程中自动起作用。

如果不加等待时间，则有可能在查找元素等操作时碰到元素尚未加载出来就会导致抛出未找到的异常影响自动化效率</i></h6>

```python
###隐式等待 driver.implicitly_wait()参数为需要等待的秒数（整型）
    from selenium import webdriver

    driver = webdriver.Chrome()
    driver.implicitly_wait(10)#隐式等待时间设置为10秒

    driver.get("http://www.jianshu.com")  # 打开简书

    container = driver.find_element_by_id("list-container")
    print(container)

###显式等待
    from selenium import webdriver
    from selenium.webdriver.support.wait import WebDriverWait
    from selenium.webdriver.support import expected_conditions as EC
    from selenium.webdriver.common.by import By

    driver = webdriver.Chrome()
    driver.implicitly_wait(10)

    driver.get("http://www.jianshu.com")  # 打开简书

    # container = driver.find_element(By.ID,"list-container") # 隐式等待
    # print(container)

    #对这次查找设置显式等待时间
    container = WebDriverWait(driver,10).until(EC.presence_of_element_located((By.ID,'list-container')))

    print(container)

###显式等待模块说明：
    #webdriverwait为显式等待方法
    from selenium.webdriver.support.wait import WebDriverWait
    #EC为条件判断方法，搭配显式等待使用
    from selenium.webdriver.support import expected_conditions as EC
    #By用于查找元素时使用
    from selenium.webdriver.common.by import By

    #webdriverwait()的构造函数如下：
    def __init__(self, driver, timeout, poll_frequency=POLL_FREQUENCY, ignored_exceptions=None)
    #driver:webdriver对象实例
    #timeout:显式等待时间
    #poll_frequency:轮询时间间隔(调用until,until_not中的方法的时间间隔，默认为0.5秒)
    #ignore_exceptions:要忽略的的异常，可以赋值为None(有异常就中断)、[NoSuchElementException]，这意味着如果在等待期间捕获到NoSuchElementException异常，等待将继续，直到元素可见或超时。
###webdriverwait对象有两种等待方式：until,until_not，它们的参数都一样：
###method:在等待期间每隔一段时间(前面设置的轮询时间)调用传入的方法
###messsage:抛出TimeoutError异常时的错误提示

###其中method参数可传入的值为expected_conditions模块中的各种条件或者是webElement的is_display()、is_enable()、is_select()方法
###expected_conditions模块中包含各种用于判断的条件例如presence_of_element_located，表示用于判断某个元素是否加载到dom树中
```
