# Python多线程
<i>参考文章：[python多线程详解（超详细）](https://blog.csdn.net/weixin_40481076/article/details/101594705) 、[Python线程池(thread pool)创建及使用+实例代码](https://blog.csdn.net/master_hunter/article/details/125070310?ops_request_misc=&request_id=&biz_id=102&utm_term=python%E7%BA%BF%E7%A8%8B%E6%B1%A0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-125070310.142^v92^chatsearchT0_1&spm=1018.2226.3001.4187) 、[第二十章 多线程](https://www.cnblogs.com/shiruiyang/p/16828536.html)</i>
<br>


 [ 1、多线程的概念 ](#1) <br>
 [ 2、python多线程的基本使用方法 ](#2) <br>
 [ 3、多线程的优点及与多进程的关系 ](#3) 
 
 <br>

  
--------

<h3 id = "1">1、多线程的概念</h3>
<br>

线程也叫轻量级进程，是操作系统能够进行运算调度的最小单位，它被包涵在进程之中，是进程中的实际运作单位。线程自己不拥有系统资源，只拥有一点在运行中必不可少的资源，但它可与同属一个进程的其他线程共享进程所拥有的全部资源。一个线程可以创建和撤销另一个线程，同一个进程中的多个线程之间可以并发执行。

<br>

<h3 id = "2">2、python多线程的基本使用方法</h3>
<br>
    
- Thread(target,args)方法创建线程

  
'''

    import threading <br>
    from threading import Lock,Thread <br>
    import time,os <br>
    #后面的代码默认都导入了这些库 
    def fun(n): 
        print('task',n)  
  
        
    if __name__ == '__main__': <br>
        t1 = threading.Thread(target = fun,1) #创建线程t1
        t2 = threading.Thread(target = fun,2) #创建线程t2
        t1.start()   #启动t1线程
        t2.start()   #启动t2线程
'''
  

- setDaemon(True)创建守护线程

'''

    def fun(n):
        print(f'task\n{n}\n')
        time.sleep(1) 
        #这里暂停1s是为了有足够时间进入另一个线程让现象更明显
        print('1s')

    if  __name__ == '__main__':
        t = threading.Thread(target=fun,args=('t1',))
        t.setDaemon(True)
        t.start()
        print('end')

'''
<br>
结果如下：<br>
task<br>
t1<br>
end<br>

这里主线程为'main'的线程，t线程为守护线程，当主线程结束时（这里打印'end'后就没有代码了）守护线程也跟着结束（print('1s')没有执行）
<br><br>

- join()方法让主线程等待子线程执行
  <br>

'''

    def fun(n):
        print(f'task\n{n}\n')
        time.sleep(1) 
        #这里暂停1s是为了有足够时间进入另一个线程让现象更明显
        print('1s')

    if  __name__ == '__main__':
        t = threading.Thread(target=fun,args=('t1',))
        t.setDaemon(True) 
        t.start()
        t.join()
        print('end')

'''

<br>
结果如下：<br>
task<br>
t1<br>
1s<br>
end<br><br>
由结果可看出即使子线程中间停止了1s，却也没有切换到主线程，而是等待子线程结束后再切换回主线程。<br><br>

- Lock()方法给线程上锁
  <br><br>
  <i>预备知识：线程是进程的执行单元，进程时系统分配资源的最小执行单位，所以在同一个进程中的多线程是共享资源的。由于线程之间是进行随机调度的，如果有多个线程同时操作一个对象，如果没有很好地保护该对象，会造成程序结果的不可预期，
  我们因此也称为“线程不安全”。为了防止上面情况的发生，就出现了互斥锁（Lock）</i>
<br><br>

未上锁情况：<br>

'''

    g_num = 100
    def work1():
        global  g_num
        time.sleep(0.1) #停止0.1s使线程切换用time.sleep(0)也可以用于表示线程被类似io调用等情况导致的阻塞或者TIK达到阈值的情况
        print('in work1 g_num is : %d' % g_num)
    

    def work2():
        global  g_num
        for i in range(100):
            g_num+=1
        print('in work2 g_num is : %d' % g_num)

    if __name__ == '__main__':
        t1 = threading.Thread(target=work1)
        t2=threading.Thread(target=work2)
        t1.start()  
        t2.start()


''' <br>
结果为：<br>
in work2 g_num is : 200<br>
in work1 g_num is : 200

很明显本来t1是先执行的线程所以打印的结果本来应该是t1线程中的print('in work1 g_num is : %d' % g_num)且结果也本应该是100但是，由于在打印前加了一个停止0.1s，导致还没打印就切换到另一个进程了（这里是t2），最终结果便也与预计的逻辑不同了。

加上互斥锁后：<br>

'''

    g_num = 100
    def work1():

        lock.acquire() #请求获取锁（如果已经有别的程序获取过了，那这一步就无法成功获取，会直接切换线程）
        global  g_num
        time.sleep(0.1)
        print('in work1 g_num is : %d' % g_num)
        lock.release() #释放锁，让其他线程可以获取到资源防止死锁。
        #Lock()方法的使用也可以使用with语句简化如下：
        #with lock:
        #    global  g_num
        #    time.sleep(0.1)
        #    print('in work1 g_num is : %d' % g_num)


    def work2():
        global  g_num
        lock.acquire()
        for i in range(100):
            g_num+=1
            #time.sleep(0.1)
        print('in work2 g_num is : %d' % g_num)
        lock.release()

    if __name__ == '__main__':
        lock = Lock()
        line = []
        t1 = threading.Thread(target=work1)
        t2 = threading.Thread(target=work2)
        line.append(t1)
        line.append(t2)
        t1.start()  
        t2.start()
        t1.join()
        t2.join()

'''
<br>
结果为：<br>
in work1 g_num is : 100<br>
in work2 g_num is : 200<br>
<br>
这里由于t1线程中已经获取了锁（lock.acquire()）在切换到t2线程时t2线程就会获取失败从而又切换回t1线程，最终结果就是先打印先获取到锁的t1线程中的字符。

- RLock()方法创建重入锁
  <br><br>
  当遇到嵌套的情况比如递归，函数间的互相调用等的时候可以使用RLock()方法创建重入锁。例如下面的程序：<br>

'''

    rlock = threading.RLock()

    def func():
        if rlock.acquire():   # 第一把锁
            print("first lock")
            if rlock.acquire():  # 第一把锁没解开的情况下接着上第二把锁
                print("second lock")
                func2()
                rlock.release()  # 解开第二把锁
            rlock.release()  # 解开第一把锁

    def func2():
        if rlock.acquire():   # 第3把锁
            print("third lock")
            if rlock.acquire():  # 第1、2、3把锁没解开的情况下接着上第4把锁
                print("fourth lock")
                rlock.release()  # 解开第4把锁
            rlock.release()  # 解开第3把锁


    t = threading.Thread(target=func)
    t.start()

<br>
结果为：<br>
   first lock<br>
   second lock<br>
   third lock<br>
   fourth lock<br>
   <br>
   Lock与RLock的区别：<br>
   ①Lock在锁定时不论是否是同一个线程在释放锁前都不能再获得锁，而RLock在锁定时同一个线程可以再获得锁（即多重上锁）<br>
   ②Lock在锁定时不属于特定线程，也就是说，Lock可以在一个线程中上锁，在另一个线程中解锁。而对于RLock来说，只有当前线程才能释放本线程上的锁
   <br><br>

- 用于开门的信号量semaphore(BoundedSemaphore类)<br>
  如果说互斥锁是用来给厕所门上锁让其它线程不能进来，那么信号量机制就是打开厕所门让指定数量的人进来。
  参考代码如下：<br><br>

'''

    def run(n,semaphore):
        semaphore.acquire()   #加锁
        print('run the thread:%s\n' % n)
        time.sleep(1) #切换线程
        print('the thread:%s is done\n' % n)
        semaphore.release()    #释放

    if __name__== '__main__':
        num=0
        semaphore = threading.BoundedSemaphore(5)   #最多允许5个线程同时运行
        for i in range(11):
            t = threading.Thread(target=run,args=('t-%s' % i,semaphore))
            t.start()
        while threading.active_count() !=1:
            pass
        else:
            print('----------all threads done-----------')

<br><br>
运行结果：<br>
run the thread:t-0<br>
run the thread:t-1<br>
run the thread:t-2<br>
run the thread:t-3<br>
run the thread:t-4<br>
the thread:t-4 is done<br>
the thread:t-2 is done<br>
the thread:t-1 is done<br>
the thread:t-3 is done<br>
the thread:t-0 is done<br>
<br>
run the thread:t-7<br>
run the thread:t-8<br>
run the thread:t-5<br>
run the thread:t-6<br>
run the thread:t-9<br>
the thread:t-5 is done<br>
the thread:t-6 is done<br>
the thread:t-9 is done<br>
run the thread:t-10<br>
the thread:t-8 is done<br>
the thread:t-7 is done<br>
<br>
the thread:t-10 is done<br>
----------all threads done----------- <br><br>

（每次运行结果都不一定相同）可以发现不论怎样，同时在运行的线程都只会有5个，满了五个后只能等其中的某个结束才能加入新的线程<br><br>

- 线程池  <br>

线程过多会带来调度开销，进而影响缓存局部性和整体性能。而线程池维护着多个线程，等待着监督管理者分配可并发执行的任务。这避免了在处理短时间任务时创建与销毁线程的代价。线程池不仅能够保证内核的充分利用，还能防止过分调度。线程池大小应该根据可用的CPU核数、内存大小和其他系统资源进行调整。如果系统资源充足，可以增加线程池的大小。同时也要考虑程序的性质和特性，例如CPU密集型还是IO密集型。如果程序主要涉及CPU密集型任务，线程池大小可以设置得较小，以避免过多的线程竞争CPU资源。如果程序主要涉及IO密集型任务，线程池大小可以设置较大，以便同时处理更多的任务。

- ThreadPoolExecutor类创建线程池<br>
  
'''

    from concurrent.futures import ThreadPoolExecutor
    import threading
    #创建一个包含2条线程的线程池



    def task(i):
        sleep_seconds = 0.5    #随机睡眠时间
        print('线程名称：%s,参数:%s,睡眠时间:%s' % (threading.current_thread().name, i, sleep_seconds))
        time.sleep(sleep_seconds)   #定义睡眠时间

    if __name__ == '__main__':
        pool = ThreadPoolExecutor(max_workers = 2)  #定义两个线程
        for i in range(10):    #创建十个任务
            future1 = pool.submit(task, i)
            #提交需要执行的程序到线程池中并返回一个future对象。Future 类主要用于获取线程任务函数的返回值。由于线程任务会在新线程中以异步方式执行，因此，线程执行的函数相当于一个 "将来完成" 的任务，所以 Python 使用 Future 来代表。
        # 关闭线程池
        #pool.shutdown()

- map(func, *iterables, timeout=None, chunksize=1)方法<br>
  
除了submit，ThreadPoolExecutor还提供了map函数来添加线程，与常规的map类似，区别在于线程池的 map() 函数会为 iterables 的每个元素启动一个线程，以并发方式来执行 func 函数. 同时，使用map函数，还会自动获取返回值。

'''

    from concurrent.futures import ThreadPoolExecutor
    import threading
    import time

    def fun(item):
        print(f'this is threading-{item}\n')
        time.sleep(0.1)
        print(f'threading-{item} is done\n')

        # 创建一个包含4条线程的线程池
    if __name__ == '__main__':
        with ThreadPoolExecutor(max_workers=4) as pool:
            results = pool.map(fun, (1,2,3))
        print('-------all threading is done---------\n')





<h3 id = "3">3、多线程的优点及与多进程的关系</h3> <br>

<i>引用文章：[第二十章 多线程](https://www.cnblogs.com/shiruiyang/p/16828536.html) 、[python多线程详解（超详细）](https://blog.csdn.net/weixin_40481076/article/details/101594705) </i><br>

- 进程

进程（Process，有时被称为重量级进程）是程序的一次执行。每个进程都有自己的地址空间、内存、数据栈以及记录运行轨迹的辅助数据，操作系统管理运行的所有进程，并为这些进程公平分配时间。进程可以通过fork和spawn操作完成其他任务。<br>

因为各个进程有自己的内存空间、数据栈等，所以只能使用进程间通信（Inter Process Communication, IPC），而不能直接共享信息。<br><br>

- 线程

线程（Thread，有时被称为轻量级进程）跟进程有些相似，不同的是所有线程运行在同一个进程中，共享运行环境。

线程有开始、顺序执行和结束3部分，有一个自己的指令指针，记录运行到什么地方。线程的运行可能被抢占（中断）或暂时被挂起（睡眠），从而让其他线程运行，这叫作让步。

一个进程中的各个线程之间共享同一块数据空间，所以线程之间可以比进程之间更方便地共享数据和相互通信。

线程一般是并发执行的。正是由于这种并行和数据共享的机制，使得多个任务的合作变得可能。实际上，在单CPU系统中，真正的并发并不可能，每个线程会被安排成每次只运行一小会儿，然后就把CPU让出来，让其他线程运行。在进程的整个运行过程中，每个线程都只做自己的事，需要时再跟其他线程共享运行结果。多个线程共同访问同一块数据不是完全没有危险的，由于访问数据的顺序不一样，因此有可能导致数据结果不一致的问题，这叫作竞态条件。大多数线程库都带有一系列同步原语，用于控制线程的执行和数据的访问。

- 什么情况下用多线程并发执行

多线程在切换中又分为I/O切换和时间切换。如果任务属于是I/O密集型，若不采用多线程，我们在进行I/O操作时，势必要等待前面一个I/O任务完成后面的I/O任务才能进行，在这个等待的过程中，CPU处于等待
状态，这时如果采用多线程的话，刚好可以切换到进行另一个I/O任务。这样就刚好可以充分利用CPU避免CPU处于闲置状态，提高效率。

但是如果多线程任务都是计算型，CPU会一直在进行工作，直到一定的时间后采取多线程时间切换的方式进行切换线程，此时CPU一直处于工作状态，此种情况下并不能提高性能，相反在切换多线程任务时，可能还会造成时间和资源的浪费，导致效能下降。这就是造成上面两种多线程结果不能的解释。
<br><br>

- GIL 全局解释器 <br>
  
GIL的全称是全局解释器，来源是python设计之初的考虑，为了数据安全所做的决定。某个线程想要执行，必须先拿到GIL，我们可以
把GIL看做是“通行证”，并且在一个python进程之中，GIL只有一个。拿不到线程的通行证，并且在一个python进程中，GIL只有一个，
拿不到通行证的线程，就不允许进入CPU执行。GIL只在cpython中才有，因为cpython调用的是c语言的原生线程，所以他不能直接操
作cpu，而只能利用GIL保证同一时间只能有一个线程拿到数据。而在pypy和jpython中是没有GIL的python在使用多线程的时候，调用的是c语言的原生过程。

  


