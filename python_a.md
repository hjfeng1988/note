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

func1('Jack', 1, 2, 3)
```
kwds接收多余的关键字参数，存放在字典中
```
def func2(name, **kwds):
    print(kwds)

func2('Jack', a=1, b=2, c=3)
```
