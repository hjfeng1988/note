
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
