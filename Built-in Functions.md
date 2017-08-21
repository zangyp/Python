# 内置函数
### `abs(x)`
返回数字的绝对值
* x--数值表达式
***
### `int(x,base=10)`
用于将一个字符串或数字转换为整型
* x--字符串或数字
* base--进制数，默认为十进制
***
### `str(obj)`
将对象转化为便于阅读的形式
* obj--要转化的对象
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
* str--指定检索的字符串
* beg--开始索引，默认为0
* end--结束索引，默认为字符串的长度
***
