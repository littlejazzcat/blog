## python中的'*'和'**'的作用

在Python中，`*` 和 `**` 是用于函数定义和函数调用时传递可变数量的参数的特殊符号。

`*` 的作用：
- 在函数定义时，`*` 可以用来在形参列表中接收任意数量的位置参数，将它们作为一个元组传递给函数体内的代码。这样定义的参数被称为可变位置参数或者是 *args 参数。
- 在函数调用时，`*` 可以用来将一个可迭代对象（如列表或元组）拆分为多个独立的位置参数，将它们传递给函数。这被称为解包操作。

下面是一些示例：

```python
# 函数定义时使用 *
def sum_numbers(*args):
    total = 0
    for num in args:
        total += num
    return total

print(sum_numbers(1, 2, 3))  # 输出：6
print(sum_numbers(4, 5, 6, 7))  # 输出：22


# 函数调用时使用 *
numbers = [1, 2, 3, 4, 5]
print(*numbers)  # 输出：1 2 3 4 5


# 结合固定参数和可变位置参数使用 *
def greet(name, *greetings):
    for greeting in greetings:
        print(greeting, name)

greet('Alice', 'Hello', 'Bonjour', 'Hola')
# 输出：
# Hello Alice
# Bonjour Alice
# Hola Alice
```

`**` 的作用：
- 在函数定义时，`**` 可以用来接收任意数量的关键字参数，将它们作为一个字典传递给函数体内的代码。这样定义的参数被称为可变关键字参数或者是 **kwargs 参数。
- 在函数调用时，`**` 可以用来将一个字典拆分为多个独立的关键字参数，将它们传递给函数。

下面是一些示例：

```python
# 函数定义时使用 **
def print_person_info(**kwargs):
    for key, value in kwargs.items():
        print(key, ":", value)

print_person_info(name='Alice', age=25, occupation='Engineer')
# 输出：
# name : Alice
# age : 25
# occupation : Engineer


# 函数调用时使用 **
person = {'name': 'Bob', 'age': 30, 'occupation': 'Teacher'}
print_person_info(**person)
# 输出：
# name : Bob
# age : 30
# occupation : Teacher


# 结合固定参数和可变关键字参数使用 **
def greet_person(name, **greetings):
    for key, value in greetings.items():
        print(value, name)

greet_person('Alice', greeting1='Hello', greeting2='Bonjour')
# 输出：
# Hello Alice
# Bonjour Alice
```

