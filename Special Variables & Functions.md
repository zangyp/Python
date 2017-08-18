# 特殊变量&函数
### `__slots__`
用于限制实例的属性，在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性
试图绑定其他属性将得到AttributeError的错误，`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的
除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`
```python
class Student(object):
     __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
***
### 
