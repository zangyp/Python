# 特殊变量&函数
## 变量
### `__slots__`
用于限制实例的属性，在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性
试图绑定其他属性将得到AttributeError的错误，`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的
除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`
```python
class Student(object):
     __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
***
## 函数
### `__str__()`&`__repr__()`
* 相同点：都是用于用于返回对象的字符串表达式 
* 不同点：`__str__()`返回用户看到的字符串（执行print时会调用），而`__repr__()`返回程序开发者看到的字符串
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```
***
### 
