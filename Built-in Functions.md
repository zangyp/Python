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
因此重复调用`print`将会在相互独立的一行中分别打印,为防止打印过程中出现这一换行符，可以通过 end 指定其应以空白结尾：  
```python
print('a', end='')
print('b', end='') # 输出为：ab
```
***
