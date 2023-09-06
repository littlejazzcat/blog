## 关于python中的值传递与引用问题

- 首先看下面这段代码
  
'''

    original_list = [1, 2, [3, 4]]

    copied_list = original_list
    copied_list[2][0] = 'a'

    print(original_list)

输出结果为<code>[1,2,['a',4]]</code>

这里本意是想为<code>original_list</code>复制一个副本后使用从而不更改原始列表的值，但是发现这个复制的副本的值改变后原始列表的值也发生改变了。<br>

字典也有同样的情况：
'''

    original_dict = {'a':12,'b':'string'}
    copied_dict = original_dict
    copied_dict['a'] = 34
    print(original_dict)

输出结果为<code>{'a':34, 'b':'string'}</code>

但是对于不可变对象例如整型变量等就没有这种情况：
'''

    original_num = 12
    copied_num = original_num
    copied_num = 3
    print(original_num)

输出结果为12.

出现这种情况的原因在于python对于可变对象（列表、字典）和不可变对象（整型、字符串...）的传递处理是不同的。

- 对于可变对象（例如列表、字典、集合等），它们的值是可以被修改的。当你将可变对象传递给一个函数或将其赋值给另一个变量时，实际上是将对象的引用传递给了函数或变量。这意味着对对象进行的修改在所有引用它的地方都是可见的，因为它们引用的是同一个对象。（这一点和C语言中的指针传递类似）

- 对于不可变对象（例如整数、字符串、元组等），每当你对其进行修改时，实际上是创建了一个新的对象，并将新对象的引用赋值给变量。这是因为不可变对象的值是不可更改的，为了在修改时保持数据的不变性，Python会创建一个新的对象来存储修改后的值。


- 解决方法
  
1、使用copy模块：
'''

    import copy

    original_list = [1, 2, [3, 4]]
    copied_list = copy.deepcopy(original_list)

    copied_list[2][0] = 'a'

    print(original_list)  # 输出：[1, 2, [3, 4]]
    print(copied_list)   # 输出：[1,2,['a',4]]

2、不使用引用
'''

    original_list = [1, 2, [3, 4]]
    copied_list = []
    #copied_list = original_list
    copied_list  += original_list +['a']

    print(original_list) #输出：[1,2,[3,4]]
    print(copied_list) #输出：[1,2,[3,4],'a']

3、注意python中的'+='

- 先看下面两段代码：

'''

    original_list = [1, 2, [3, 4]]
    #copied_list = []
    copied_list = original_list
    copied_list  += ['a']

    print(original_list) #输出：[1,2,[3,4],'a']
    print(copied_list) #输出：[1,2,[3,4],'a']

'''

    original_list = [1, 2, [3, 4]]
    #copied_list = []
    copied_list = original_list
    copied_list  = copied_list + ['a']

    print(original_list) #输出：[1,2,[3,4]]
    print(copied_list) #输出：[1,2,[3,4],'a']

这两段代码逻辑基本一致，唯一的区别就是前者使用了'+='，后者则在逻辑上将'+='拆开了（copied_list = copied_list + ['a']），如果这是在C语言中那这两者的结果应该是一样的，但是python3中对于可变对象的'+='和将其拆开的逻辑是有区别的
<font color = red>使用 '+=' 会在原始对象上进行就地修改，而 x = x + ... 会创建一个新的对象，并将其赋值给变量 x</font>

- 看下面两段代码

'''

    original_list = [1, 2, [3, 4]]
    #copied_list = []
    copied_list = original_list
    print(id(original_list)) #输出2429054333312
    print(id(copied_list)) #输出2429054333312
    copied_list  = copied_list + ['a']
    print(id(copied_list)) #输出2429054455040
    #具体数字不重要，重要的是两个地址的值不同了

'''

    original_list = [1, 2, [3, 4]]
    #copied_list = []
    copied_list = original_list
    print(id(original_list)) #输出2487616975360
    print(id(copied_list)) #输出2487616975360
    copied_list  += ['a']
    print(id(copied_list)) #输出2487616975360
    #可以看到地址没有发生变化

- 再补充一点，关于python中的不可变对象。在python中这些不可变对象本身就存在于固定的地址空间中（类似于C语言的常数）而在引用不可变对象时实际上是将变量指向这些地址空间，可以看下面的代码：
  
'''

    original_num = 12
    copied_num = original_num
    print(id(12))
    print(id(original_num))
    print(id(copied_num))
    #三者输出一致（140711921968264），且不论运行几次结果都一样
    copied_num = 3
    print(id(original_num))#输出：140711921968264
    print(id(3))
    print(id(copied_num))
    #二者输出一致（140711921967976）且不论运行几次结果都一样