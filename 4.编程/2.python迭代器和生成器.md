# 4.类，可迭代对象，迭代器
类中定义了`__iter__()`方法，实例化后的对象就是可迭代的，`iter()`将可迭代对象转化为迭代器；

类中同时定义了`__iter__()`和`__next__()`方法，实例化后的对象就是迭代器。

for loop作用于可迭代对象或迭代器，就是一次次调用`next(obj)`或者`obj.__next__()`，最后抛出stopIteration。

可迭代对象
```
from collections.abc import Iterable, Iterator
class IterObj:
    def __init__(self, data):
        self.data = data
    def __iter__(self):
        return iter(self.data)
it = IterObj((2, 3, 4))
print(isinstance(it, Iterable))
print(isinstance(it, Iterator))

for i in it:
    print(i)
```
迭代器
```
from collections.abc import Iterable, Iterator
class IterObj:
    value = 0
    def __init__(self, data):
        self.data = data
    def __iter__(self):
        return self
    def __next__(self):
        if self.value >= len(self.data):
            raise StopIteration
        ret = self.data[self.value]
        self.value += 1
        return ret
it = IterObj((2, 3, 4))
print(isinstance(it, Iterable))
print(isinstance(it, Iterator))

for i in it:
    print(i)
```

# 5.生成器
生成器是一个用于创建迭代器的工具，会自动创建`__iter__()`和`__next__()`方法，有两种形式：

* 定义带有yeild的函数
* 生成器表达式（语法类似于列表推导式）
