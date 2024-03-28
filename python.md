# 1.数值，字符串，列表，元组，字典，集合

# 2.输出格式化

有三种方式：%|.format|f''

```
name = 'Mike'
age = 35

print('My name is %s, I am %d years old.' % (name, age))
print('My name is {}, I am {} years old.'.format(name ,age))
print(f'My name is {name}, I am {age} years old.')
```

# 3.函数，位置参数，关键字参数
args接收多余的位置参数，存放在元组中，可以与kwds同时使用
```
def func1(name, *args):
    print(args)
```
kwds接收多余的关键字参数，存放在字典中
```
def func2(name, **kwds):
    print(kwds)
```

# 4.类，迭代器
类中定义了`__iter__()`方法，实例化后的对象就是可迭代的，`iter()`将可迭代对象转化为迭代器；

类中同时定义了`__iter__()`和`__next__()`方法，实例化后的对象就是迭代器。

for loop作用于可迭代对象或迭代器，就是一次次调用`next(obj)`或者`obj.__next__()`，最后抛出stopIteration。

# 5.生成器
生成器是一个用于创建迭代器的工具，会自动创建`__iter__()`和`__next__()`方法，有两种形式：

* 定义带有yeild的函数
* 生成器表达式（语法类似于列表推导式）


# 6.装饰器，函数装饰器，带参数的函数装饰器
函数装饰器
```
def dec1(func):
    def inner_func():
        print('每个被装饰的函数都会打印我')
        return func()
    return inner_func

@dec1 # 等同于func1 = dec(func1)
def func1():
    print('I am func1')

func1()
```
带参数的函数装饰器
```
def dec2(level):
    def wrapper(func):
        def inner_func():
            print(f'[{level}]: 每个被装饰的函数都会打印我')
            return func()
        return inner_func
    return wrapper

@dec2('INFO') # 等同于func2 = dec2('INFO')(func2)
def func2():
    print('I am func2')
    
func2()
```
# 7.装饰器，类装饰器，带参数的类装饰器
类装饰器
```
class A:
    def __init__(self, f):
        print("inside A.__init__()")

    def __call__(self):
        print("inside A.__call__()")

@A
def func1():
    print("inside func1()")


print("Finished decorating func1()")
func1()
```
带参数的类装饰器
```
class B:
    def __init__(self, level):
        print("inside B.__init__()")
        self.level = level

    def __call__(self, func):
        print(f"[{self.level}]:inside B.__call__()")
        def wrapper():
            print(f"[{self.level}]: inside wrapper()")
            return func()
        return wrapper

@B('DEBUG')
def func2():
    print("inside func2()")

print("Finished decorating func2()")
func2()
```

# 8.类，实例方法，类方法，静态方法
```
class MyClass:
    def instance_method(self):
        print('This is a instance method')
    
    @classmethod
    def class_method(cls):
        print('This is a class method')
    # class_method = classmethod(class_method)

    @staticmethod
    def static_method():
        print('This is a static method')
    # static_method = staticmethod(static_method)
```

实例方法：
> 实例后，通过`obj.method()`或者`Class.method(obj)`调用
```
c = MyClass()
c.instance_method()
MyClass.instance_method(c)
```


类方法：
> 不必实例化，可通过类直接调用
```
MyClass.class_method()
c = MyClass()
c.class_method()
```


静态方法：
> 不需要传入self和cls，不能调用类中定义的属性和其它方法
```
MyClass.static_method()
c = MyClass()
c.static_method()
```
