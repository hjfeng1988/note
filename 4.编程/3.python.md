
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
