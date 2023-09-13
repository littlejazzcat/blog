## python中的闭包

[1、什么是闭包](#1)

[2、闭包的作用域问题](#2)

[3、闭包的应用场景](#3)

---

<h3 id = 1>1、什么是闭包</h3>

闭包指的是闭包函数，与函数嵌套的区别在于闭包函数（内层函数）会使用到外层函数的变量。

- 构成闭包的几个条件

1、一个函数必须有一个内层函数；

2、内层函数必须使用到外层函数的变量；

3、外层函数必须返回内层函数。

例如：

```python
def outer_func(x):
    a = 1
    b = 2
    def inner_func(y):
        return x + y + a + b
    return inner_func

closure_func = outer_func(10) #给外部函数传入参数10
print(closure_func.__closure__)  # 输出 (<cell at 0x7f856b37e9c0: int object at 0x7f856b37e9c0>,)
cell0 = closure_func.__closure__[0] 
cell2 = closure_func.__closure__[1] 
cell3 = closure_func.__closure__[2] 
print(cell0.cell_contents,cell2.cell_contents,cell3.cell_contents) #输出1,2,10

print(closure_func(5)) #给内层函数传入参数5
```

<h3 id = 2>2、闭包的作用域问题</h3>

在分析python函数闭包的作用域问题前首先要知道python中函数对象的一个属性：<code>\__closure__</code>
- 在 Python 中，__closure__ 是一个内置属性，用于访问一个闭包（closure）的外部变量（自由变量）。

从上面的例子中可以看到在outer_func函数被调用完后，通过访问__closure__属性得到了一个元组这个元组中存放的就是闭包的内层函数使用到的外层函数的变量，后面通过cell_contents方法访问元组中的每个元素分别得到了1,2,10，这三个值正好是外层函数的a,b,x变量。

- 可以理解成在使用闭包函数时，python会自动将内层函数所使用到的外层函数的变量放入调用闭包函数的对象的__closure__属性对应的元组中如上面例子的closure.func.\__closure__

<h3 id = 3>3、闭包的应用场景</h3>

- 1、装饰器

```python
def logger(func):
    def wrapper(*args, **kwargs):
        print("调用函数: ", func.__name__)
        return func(*args, **kwargs)
    return wrapper

@logger
def add(x, y):
    return x + y

result = add(2, 3)  # 输出：调用函数: add
print(result)  # 输出：5
```

- 2、延迟执行

```python
def lazy_evaluation():
    result = None

    def calculate():
        nonlocal result
        if result is None:
            result = 10 + 20  # 这里可以是复杂的计算过程
        return result

    return calculate

lazy_calculation = lazy_evaluation()

# ...

result = lazy_calculation()  # 真正执行计算，返回结果
print(result)  # 输出：30
```

- 3、计数器

```python
def counter():
    count = 0

    def increment():
        nonlocal count
        count += 1
        return count

    return increment

counter1 = counter()
print(counter1())  # 输出：1
print(counter1())  # 输出：2

counter2 = counter()
print(counter2())  # 输出：1
```

- 4、缓存

```python
def cache():
    cached_results = {}

    def calculate_or_get_cached_result(x):
        if x in cached_results:
            return cached_results[x]
        else:
            result = x * 2  # 这里可以是复杂的计算过程
            cached_results[x] = result
            return result

    return calculate_or_get_cached_result

calculate = cache()

# ...

result1 = calculate(5)  # 第一次计算，结果将被缓存
print(result1)  # 输出：10

result2 = calculate(5)  # 直接从缓存中获取结果
print(result2)  # 输出：10
```
