# 特殊变量&函数
### __slots__
用于限制实例的属性，在定义class的时候，定义一个特殊的__slots__变量，来限制该class实例能添加的属性
```python
class Student(object):
     __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
***
### 
