# Special Attributes & Methods
Python中特殊的属性和方法
## Attributes（属性）
### `__name__`
每个模块都有一个名称，可以通过使用模块的`__name__`属性来判断它是为自己所用还是从其它从的模块中导入而来
```python
if __name__ == '__main__':
print('This program is being run by itself')
else:
print('I am being imported from another module')
```
***
### `__slots__`
用于限制实例的属性，在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性
试图绑定其他属性将得到AttributeError的错误，`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的
除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`
```python
class Student(object):
     __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
***
## Methods（方法）
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
### `__iter__()`
如果一个类想被用于`for ... in`循环，类似list或tuple那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象，然后，Python的for循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环
```python
class Fib(object): #以斐波那契数列为例
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
        
for n in Fib():
    print(n)
```
***
### `__getattr__()`
动态返回一个属性，当调用不存在的属性时，Python解释器会试图调用`__getattr__()`来尝试获得属性  
只有在没有找到属性的情况下，才调用`__getattr__`，已有的属性，比如name，不会在`__getattr__`中查找
```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```
***
### `__call__()`
直接调用对象本身，类似对一个函数进行调用，并且可以定义参数  
通过`callable()`函数，可以判断一个对象是否是“可调用”对象
```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
```
调用：
```python
s = Student('George')
s() 
```
***
### `__init__()`
`__init__`方法负责对象的初始化，系统执行该方法前，其实该对象已经存在了
```python
def __init__(self):
    print("__init__ ")
    print(self)
    super(A, self).__init__()
```
***
### `__new__()`
在创建一个实例的时候，`__new__`方法先被调用，返回一个实例对象，接着才是`__init__`被调用  
`__new__`方法的返回值就是类的实例对象，这个实例对象会传递给`__init__`方法中定义的self参数，以便实例对象可以被正确地初始化  
如果`__new__`方法不返回值（或者说返回None）那么`__init__`将不会得到调用，因为实例对象都没创建出来，调用`__init__`也没什么意义  
此外，Python还规定，`__init__`只能返回None值，否则报错  
可以用于实现单例模式：
```python
class BaseController(object):
    _singleton = None
    def __new__(cls, *a, **k):
        if not cls._singleton:
            cls._singleton = object.__new__(cls, *a, **k)
        return cls._singleton
```
### `__del__()`
这一方法在对象被删除之前调用（它的使用时机不可预测，所以避免使用它）
***
