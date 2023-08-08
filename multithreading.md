# Python多线程
<i>参考文章：[python多线程详解（超详细）](https://blog.csdn.net/weixin_40481076/article/details/101594705) </i>
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
