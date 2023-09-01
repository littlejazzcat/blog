## python Django前置知识

---

- divmod(dividend,divisor)

divmod()是Python的一个内置函数，它用于同时执行除法操作和取余操作。这个函数接受两个参数，都是整数或浮点数。<br>
其中，dividend是被除数，divisor是除数。<br>
divmod()函数返回一个元组，包含两个元素：商和余数。<br>

- collections.OrderedDict类

在 Python 中，collections.OrderedDict 是一个字典子类，它保留了插入顺序。这意味着当你遍历这个字典时，元素的顺序将与它们最初添加到字典中的顺序一致。

下面是一个简单的示例：
'''

    python
    from collections import OrderedDict

    d = OrderedDict()

    d['first'] = 1
    d['second'] = 2
    d['third'] = 3

    # 遍历字典
    for key, value in d.items():
        print(key, value)
    输出：

    sql
    first 1
    second 2
    third 3
这对于需要保持特定顺序的字典数据结构非常有用。然而，从 Python 3.7 开始，Python 的内置字典类型（dict）也保留了元素的插入顺序，因此 collections.OrderedDict 在许多情况下可能不是必需的

- 字典的默认值如何设置

1、直接赋值：你可以在创建字典时直接为键设置默认值。如果尝试访问不存在的键，将会返回这个默认值。
'''

    default_dict = {key1: value1, key2: value2, ...}

2、使用 dict.get() 方法：这个方法允许你为不存在的键提供一个默认值。如果访问的键不存在，将会返回这个默认值。
'''

    dict = {'key1': 'value1', 'key2': 'value2'}
    default_value = dict.get('non_existent_key', 'default_value')
在这个例子中，如果'non_existent_key'不存在，dict.get()将返回'default_value'。

- collections.deque双端队列

collections.deque 是 Python 的一个内置模块，它提供了一个双端队列（double-ended queue）。双端队列是一种数据结构，可以在队列的两端进行插入和删除操作。collections.deque 提供了一些基本操作，例如 append(), appendleft(), pop(), popleft() 等。

下面是一个简单的例子：
'''

    from collections import deque

    # 创建一个双端队列
    dq = deque()

    # 在队列的右侧添加元素
    dq.append('a')
    dq.append('b')

    # 在队列的左侧添加元素
    dq.appendleft('c')

    print(dq)  # 输出：deque(['c', 'a', 'b'])

    # 从队列的右侧移除元素
    right_element = dq.pop()
    print(right_element)  # 输出：'b'
    print(dq)  # 输出：deque(['c', 'a'])

    # 从队列的左侧移除元素
    left_element = dq.popleft()
    print(left_element)  # 输出：'c'
    print(dq)  # 输出：deque(['a'])
此外，collections.deque 还支持线程安全，可以在多线程环境中使用。如果需要在多线程环境中使用队列，那么 collections.deque 是一个很好的选择。

- 变量查找顺序LEGB

LEGB分别代表四种作用域：Local（L）、Enclosing（E）、Global（G）、Builtin（B）。

Python在函数中查找变量时会按照L、E、G、B的顺序进行查找，首先在函数内部（Local）查找，然后到函数内部与嵌套函数之间（Enclosing）查找，再到全局（Global）查找，最后在内置作用域（Builtin）查找。

内置作用域就是Python解释器内置的一些变量和函数，比如math模块中的函数，需要使用math模块中的变量时，就可以直接调用内置作用域中的变量。

- 类中的__和_

1、双下划线__开头的成员变量或方法，是Python的特殊方法，也称为魔法方法或内置方法。例如，__init__是类的构造函数，用于初始化实例变量。<br>
2、单下划线_开头的成员变量或方法，是Python的“保护”属性或方法。它们被视为类的内部实现细节，不建议直接访问或修改。如果需要访问或修改，建议使用相应的API。
以下是示例代码：

    class MyClass:
        def __init__(self, name):
            self.__name = name  # 双下划线开头的成员变量，表示这是类的私有属性
            self._age = 20      # 单下划线开头的成员变量，表示这是类的保护属性

        def __print_name(self):  # 双下划线开头的成员方法，表示这是类的私有方法
            print(self.__name)

        def _print_age(self):    # 单下划线开头的成员方法，表示这是类的保护方法
            print(self._age)

        def print_public_name(self):  # 没有特殊字符开头的成员方法，表示这是类的公有方法
            print(self.name)

- 深复制与浅复制

在Python中，深复制（Deep Copy）和浅复制（Shallow Copy）是两种处理对象引用的方式，它们对于对象内部元素的复制有着不同的影响。

浅复制（Shallow Copy）：

浅复制只复制对象本身，而不会复制对象内部的元素。这意味着，如果对象内部是可变元素（例如列表或字典），那么修改这些元素将会影响原来的对象。例如：

    import copy

    original_list = [1, 2, [3, 4]]
    copied_list = copy.copy(original_list)

    copied_list[2][0] = 'a'

    print(original_list)  # 输出：[1, 2, ['a', 4]]
可以看到，修改了copied_list的第三个元素（也是一个列表）的第一个元素后会影响到original_list。

深复制（Deep Copy）：

深复制会复制对象以及对象内部的所有元素。这意味着，无论对象内部的元素是否可变，都不会影响到原来的对象。例如：

    import copy

    original_list = [1, 2, [3, 4]]
    copied_list = copy.deepcopy(original_list)

    copied_list[2][0] = 'a'

    print(original_list)  # 输出：[1, 2, [3, 4]]
这次我们修改了copied_list的第三个元素（也是一个列表）的第一个元素，但是这并不会影响到original_list。

- exec、eval、repr函数

1、exec: exec函数用于执行存储在字符串或对象中的Python代码。它允许动态生成和执行代码。exec函数不返回任何结果，它的主要用途是执行那些在运行时才能确定的代码。例如，它常常用于实现编程语言解释器，或者是在运行时根据某些条件动态改变程序的行为。

    code = """
    def square(x):
        return x * x
    """
    exec(code)
    print(square(10))  # 输出：100
2、eval: eval函数用于评估一个包含单个Python表达式的字符串，并返回表达式的值。换句话说，eval可以将字符串转换为Python表达式并求值。这使得它非常适合用于动态计算和解析用户输入的表达式。

    expression = "2 + 2 * 3"
    result = eval(expression)
    print(result)  # 输出：8
3、repr: repr函数返回一个对象的“官方”字符串表示，这对于打印调试信息或需要了解对象完整信息的场合非常有用。它通常返回一个可以用来重建该对象的字符串（即，如果eval(repr(obj))，则应该返回原对象）。对于一些内置类型的对象（如字符串、列表等），repr返回的字符串通常是该类型的构造器调用形式。

    x = 12345
    print(repr(x))  # 输出：'12345'

- pickle、json、shutil模块

1、pickle模块可以将Python对象序列化为二进制格式，以便在文件或网络上进行传输或存储。它也可以将二进制数据反序列化回Python对象。pickle模块提供了一个非常灵活的方式，可以将几乎所有的Python对象（如列表、字典、类实例等）序列化为二进制格式。

以下是pickle模块的用法：

    import pickle

    # 对象序列化为二进制数据并写入文件
    data = { 'name': 'Alice', 'age': 25 }
    with open('data.pickle', 'wb') as f:
        pickle.dump(data, f)

    # 从文件中读取二进制数据并反序列化为对象
    with open('data.pickle', 'rb') as f:
        loaded_data = pickle.load(f)
    print(loaded_data)  # 输出：{'name': 'Alice', 'age': 25}
pickle模块提供了一些方法用于序列化和反序列化Python对象，如dump()和load()。其中，dump()方法将Python对象序列化为二进制数据并写入文件，而load()方法从文件中读取二进制数据并反序列化为Python对象。

需要注意的是，pickle模块在序列化和反序列化时需要保证安全性。因此，不应该使用pickle模块来反序列化不受信任的二进制数据，因为这可能会导致代码执行任意操作。

2、json模块可以将Python对象转换为JSON格式的字符串，以便在网络上进行传输或存储。它也可以将JSON格式的字符串转换回Python对象。json模块提供了简单的API来处理JSON数据和Python对象之间的转换。

以下是json模块的用法：

    import json

    # Python对象转换为JSON格式字符串
    data = { 'name': 'Alice', 'age': 25 }
    json_str = json.dumps(data)
    print(json_str)  # 输出：'{"name": "Alice", "age": 25}'

    # JSON格式字符串转换为Python对象
    loaded_data = json.loads(json_str)
    print(loaded_data)  # 输出：{'name': 'Alice', 'age': 25}
json模块提供了一些方法用于将Python对象转换为JSON格式的字符串，如dumps()。它也提供了一些方法用于将JSON格式的字符串转换回Python对象，如loads()。需要注意的是，json模块只能处理基本的Python数据类型（如数字、字符串、列表、字典等），而不能处理一些复杂的Python对象（如类实例等）。

3、shutil模块提供了一些高级的文件操作功能，如复制、移动、删除文件等。它提供了一些方法来简化文件操作，并可以处理大文件和目录树。

以下是shutil模块的用法：

    import shutil

    # 复制文件
    shutil.copy('source.txt', 'destination.txt')

    # 移动文件
    shutil.move('source.txt', 'destination_directory/destination.txt')

    # 删除文件
    shutil.rmtree('directory')  # 删除目录及其内容
    shutil.rmtree('file.txt')  # 删除文件并保留目录结构
    shutil.unlink('file.txt')  # 删除文件但不保留目录结构
shutil模块提供了一些方法用于复制、移动、删除文件等操作。例如，copy()方法用于复制文件，move()方法用于移动文件或重命名文件，rmtree()方法用于删除目录及其内容，unlink()方法用于删除文件但不保留目录结构。需要注意的是，shutil模块在处理大文件和目录树时可以更高效地处理文件操作。