# 内置函数
### `abs(x)`
返回数字的绝对值
* x:数值表达式
***
### `int(x,base=10)`
用于将一个字符串或数字转换为整型
* x:字符串或数字
* base:进制数，默认为十进制
***
### `hex(x)`
用于将10进制整数转换成16进制整数
* x:十进制整数
***
### `sum(iterable[, start])`
对可迭代对象进行求和计算
* iterable:可迭代对象，如列表
* start:定相加的参数，如果没有设置这个值，默认为0
```python
>>> sum([0,1,2,3,4], 2)  # 列表计算总和后再加 2
12
```
***
### `pow(x, y[, z])`
计算x的y次方(等价于`x**y`)，如果z在存在，则再对结果进行取模，其结果等效于`pow(x,y) %z`  
*注意*：pow() 通过内置的方法直接调用，内置方法会把参数作为整型，而 math 模块则会把参数转换为 float
***
### `len(s)`
返回对象（字符、列表、元组等）长度或项目个数
* s:对象
***
### `range(start, stop[, step])`
创建一个整数列表，一般用在 for 循环中
* start:计数从start开始。默认是从0开始。例如`range(5)`等价于`range(0,5)`
* end:计数到end结束，但不包括end。例如：`range(0,5)`是[0,1,2,3,4]没有5
* step:步长，默认为1。例如：`range(0,5)`等价于`range(0,5,1)`
***
### `tuple(seq)`
将列表转换为元组
* seq:要转换为元组的序列
```python
>>> tuple({1:2,3:4})  #针对字典 会返回字典的key组成的tuple
(1,3)
```
***
### `list(seq)`
将元组转换为列表
* seq:要转换为列表的元组
***
### `str(obj)`
将对象转化为便于阅读的形式（返回一个对象的string格式）
* obj:要转化的对象
***
### `str.format()`
格式化方法，用于从其他信息中构建字符串
```python
age = 20
name = 'Swaroop'
print('{0} was {1} years old when he wrote this book'.format(name, age)) # {}中的数字可以不填
print('Why is {0} playing with that python?'.format(name))

# 对于浮点数 '0.333' 保留小数点(.)后三位
print('{0:.3f}'.format(1.0/3))
# 使用下划线填充文本，并保持文字处于中间位置
# 使用 (^) 定义 '___hello___'字符串长度为 11
print('{0:_^11}'.format('hello'))
# 基于关键词输出 'Swaroop wrote A Byte of Python'
print('{name} wrote {book}'.format(name='Swaroop', book='A Byte of Python'))
```
`print`总是会以一个不可见的“新一行”字符（`\n`）结尾  
因此重复调用`print`将会在相互独立的一行中分别打印，为防止打印过程中出现这一换行符，可以通过 end 指定其应以空白结尾：  
```python
print('a', end='')
print('b', end='') # 输出为：ab
```
***
### `dir([obj])`
`dir()`函数能够返回由对象所定义的名称列表。  
如果这一对象是一个模块，则该列表会包括函数内所定义的函数、类与变量。  
该函数接受参数，如果参数是模块名称，函数将返回这一指定模块的名称列表。  
如果没有提供参数，函数将返回当前模块的名称列表。
```python
$ python
>>> import sys
# 给出 sys 模块中的属性名称
>>> dir(sys)
['__displayhook__', '__doc__',
'argv', 'builtin_module_names',
'version', 'version_info']
# only few entries shown here
# 给出当前模块的属性名称
>>> dir()
['__builtins__', '__doc__',
'__name__', '__package__']
# 创建一个新的变量 'a'
>>> a = 5
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__', 'a']
# 删除或移除一个名称
>>> del a
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__']
```
***
### `str.find(str, beg=0, end=len(string))`
检测字符串中是否包含子字符串 str ，如果指定beg（开始）和end（结束）范围，则检查是否包含在指定范围内，如果包含子字符串返回开始的索引值，否则返回-1
* str:指定检索的字符串
* beg:开始索引，默认为0
* end:结束索引，默认为字符串的长度
***
### `open(name[, mode[, buffering]])`&`file(name[, mode[, buffering]])`
用于打开一个文件，创建一个file对象，相关的方法才可以调用它进行读写
* name:包含了要访问的文件所在路径（包括文件名）
* mode:决定了打开文件的模式：只读，写入，追加等，默认文件访问模式为只读(r)
  * r:以只读方式打开文件。文件的指针将会放在文件的开头，默认模式
  * rb:以二进制格式打开一个文件用于只读。文件的指针将会放在文件的开头，默认模式
  * r+:打开一个文件用于读写。文件指针将会放在文件的开头
  * rb+:以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头
  * w:打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件
  * wb:以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件
  * w+:打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件
  * wb+:以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件
  * a:打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入
  * ab:以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入
  * a+:打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写
  * ab+:以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写
* buffering:如果buffering的值被设为0，就不会有寄存；如果buffering的值取1，访问文件时会寄存行；如果将buffering的值设为大于1的整数，表明了这就是的寄存区的缓冲大小；如果取负值，寄存区的缓冲大小则为系统默认
```python
with open('/path/to/file', 'r') as f: # 这种方式不必调用f.close()
    print(f.read())
```
***
### `isinstance()`&`type()`
`type(name[, bases, dict])`如果只传入第一个参数则返回对象的类型，传入三个参数返回新的类型对象
* name:类的名称
* bases:基类的元组
* dict:字典，类内定义的命名空间变量  

`isinstance()`与`type()`区别：  
* `type()` 不会认为子类是一种父类类型，不考虑继承关系
* `isinstance()` 会认为子类是一种父类类型，考虑继承关系
如果要判断两个类型是否相同推荐使用 `isinstance()`
```python
# 一个参数实例
>>> type(1)
<type 'int'>
>>> type('runoob')
<type 'str'>
>>> type([2])
<type 'list'>
>>> type({0:'zero'})
<type 'dict'>
>>> x = 1          
>>> type( x ) == int    # 判断类型是否相等
True
 
# 三个参数
>>> class X(object):
...     a = 1
...
>>> X = type('X', (object,), dict(a=1))  # 产生一个新的类型 X
>>> X
<class '__main__.X'>
```
***
### `@classmethod`&`@staticmethod`
一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法，而使用`@staticmethod`或`@classmethod`，就可以不需要实例化，直接类名.方法名()来调用
* `@staticmethod`不需要表示自身对象的self和自身类的cls参数，就跟使用函数一样
* `@classmethod`不需要self参数，但第一个参数需要是表示自身类的cls参数
```python
class A(object):  
    bar = 1  
    def foo(self):  
        print 'foo'  
 
    @staticmethod  
    def static_foo():  
        print 'static_foo'  
        print A.bar  
 
    @classmethod  
    def class_foo(cls):  
        print 'class_foo'  
        print cls.bar  
        cls().foo()  
  
A.static_foo()  
A.class_foo()  
```
***
