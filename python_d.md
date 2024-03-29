# 8.类，实例方法，类方法，静态方法，属性方法
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

    @property
    def property_method(self):
        print('This is a property method')
        print('Don\'t need () after property_method')
```


实例方法：实例化后调用
```
c = MyClass()
c.instance_method()
MyClass.instance_method(c)
```


类方法：不必实例化，可通过类直接调用
```
MyClass.class_method()
c = MyClass()
c.class_method()
```


静态方法：不需要传入self和cls，不能调用类中定义的属性和其它方法
```
MyClass.static_method()
c = MyClass()
c.static_method()
```


属性方法：实例通过属性形式调用方法，调用时property_method不用加`()`
```
c = MyClass()
c.property_method
```