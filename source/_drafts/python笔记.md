---
title: python笔记
date: 2018-05-24 07:52:07
categories:
    - python
tags:
    - python  
---
# python笔记

## python之蝉
解释器中输入import this
Now is better than never.

## String
大写name.upper()
小写name.lower()
首字母大写name.title()
使用+拼接字符串
去掉空格strip()，lstrip()，rstrip()
将非字符串值表示为字符串str()

## 数字
在python2和python3中除的结果不一样
```
3/2=1   #python2
3/2=1.5 #python3
```

## 列表
索引为负数时列表倒数，索引-1返回最后一个元素
追加元素list.append(xx)
插入元素list.instert(2,xx)
删除元素 del list(2)
弹出元素 list.pop(),list.pop(2)
根据值删除 list.pop('lisi')
排序 list.sort()
临时排序 list.sorted()
反转顺序 list.reverse()
长度 list.len()
生成数值数组函数range()
列表解析
```
squares = [value** 2 for value in range( 1,11)]
```
遍历
for  item in  list:

## 切片
列表的部分元素
切片复制
```
my_foods = [' pizza', 'falafel', 'carrot cake'] 
friend_foods = my_foods[:]
```

## 元组
Python将不能修改的值称为不可变的，而不可变的列表被称为元组。元组不可以改变元素，但是可以重新定义元组。
定义元组使用圆括号

## for 遍历
```
for  item in  list:
    
for k, v in map.items():
```

## if
```
if true :
    ...
elif:
    ...
else:
    ...
```
检查多个条件使用and，or
检测元素是否在列表中 if item in list: ,if item not in list:

## while
在列表之间移动元素 while list:
判断列表包含某个元素 while 'aa' in list:

## 获取输入
python2 raw_input()
python3 input()

## 字典
字典是一系列键值对，用放在花括号{}中的一系列键—值对表示
删除元素 del userMap['name']
遍历字典键值 for k, v in map.items():
遍历字典中所有建 for name in favorite_languages.keys():
遍历字典中所有的值 for language in favorite_languages.values():

## 函数
定义函数使用 def
实参和形参
位置实参，关键字实参，默认值
在函数中对这个列表所做的任何修改都是永久性的
禁止函数修改列表，将列表的副本传递给函数 list[:]，用切片创建副本
传递任意数量的实参，形参名*toppings中的星号让python创建一个名为toppings的空元组，将收到的值都封装到这个元组中。形参**user_info中的两个星号让Python创建一个名为user_info的空字典，并将收到的所有名称—值对都封装到这个字典中。

## 导入函数
导入整个模块 import module_name
导入模块特定函数 from module_name import function_name1,function_name2
使用as给函数指定别名 from module_name import function_name1 as name1
使用as给模块指定别名 import module_name as m1
导入模块全部函数 from module_name import *

## 类
关键字 class 定义类
创建类dog.py 
```
class Dog(): 
    """ 一 次 模 拟 小 狗 的 简 单 尝 试""" 
    def __init__( self, name, age): 
        """ 初 始 化 属 性 name 和 age""" 
        self.name = name self.age = age

    def sit( self): 
        """ 模 拟 小 狗 被 命 令 时 蹲 下""" 
        print( self.name.title() + " is now sitting.") 

    def roll_over( self): 
        """ 模 拟 小 狗 被 命 令 时 打 滚""" 
        print( self.name.title() + " rolled over!")
```
方法__init__()，创建实例时会自动运行，这个方法的定义中，形参self必不可少，还必须位于其他形参的前面。创建实例时，将自动传入实参self。每个与类相关联的方法调用都自动传递实参self，它是一个指向实例本身的引用，让实例能够访问类中的属性和方法。以self为前缀的变量都可供类中的所有方法使用

创建类实例
```
my_dog=Dog('wille',6)
```

## 类的继承
一个类继承另外一个类时，它将自动获得另一个类的所有属性和方法。
创建子类时，父类必须包含在当前文件中，且位于子类前面。定义子类时，必须在括号内指定父类的名称。

## 文件

```
with open('pi_digits.txt') as file_obj:
    contents=file_obj.read()
```
with可以妥善管理打开和关闭

## 异常
```
try:
<语句>        #运行别的代码
except <名字>：
<语句>        #如果在try部份引发了'name'异常
except <名字>，<数据>:
<语句>        #如果引发了'name'异常，获得附加的数据
else:
<语句>        #如果没有异常发生
```

## 存储数据
json.dump(),json.load()
```
numbers =[2,3,5,7]
filename='jsondump.txt'
with open(filename,'w') as file_obj:
    json.dump(numbers,file_obj)

filename='jsondump.txt'
with open(filename,'r') as file_obj:
    numbers=json.load(numbers,file_obj)
print(numbers)

```

## 测试代码
```
import unittest
class TestDict(unittest.TestCase):
    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))
```

## pygame
linux安装
```
安装 pygame
pip install pygame
hg clone https://bitbucket.org/pygame/pygame
python3 setup.py build
sudo python3 setup.py install

```

mac安装
```
brew install hd sdl sdl_image sdl_ttf
brew install sdl_mixer  portmidi  # 声音
# pip3 install hg+http://bitbucket.org/pygame/pygame
pip3 install pygame
```

## 帮助文档
使用pydoc查看
pydoc keyword
pydoc3 keyword

查询模块的使用方法
1. 命令模式输入help()，然后输入关键字
2. 命令模式输入help(str),help(str.split)
quit退出
```
Help on class str in module builtins:

class str(object)
 |  str(object='') -> str
 |  str(bytes_or_buffer[, encoding[, errors]]) -> str
 |
 |  Create a new string object from the given object. If encoding or
 |  errors is specified, then the object must expose a data buffer
 |  that will be decoded using the given encoding and error handler.
 |  Otherwise, returns the result of object.__str__() (if defined)
 |  or repr(object).
 |  encoding defaults to sys.getdefaultencoding().
 |  errors defaults to 'strict'.
 |
 |  Methods defined here:
 |
 |  __add__(self, value, /)
 ... ...
```

查看模块下所有函数dir(module_name)，如
```
>>> dir(str)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

查看对象某个属性的帮助文档，如要查看str的split属性，可以用__doc__，print(str.split.__doc__)
```
>>> print(str.split.__doc__)
S.split(sep=None, maxsplit=-1) -> list of strings

Return a list of the words in S, using sep as the
delimiter string.  If maxsplit is given, at most maxsplit
splits are done. If sep is not specified or is None, any
whitespace string is a separator and empty strings are
removed from the result.
```


## 分界符
python的分界符是冒号和缩进

一切都是对象

异常
    
## __name__
如果导入一个模块，那么这个模块的__name__就是这个模块的文件名，如果直接运行这个模块，那么__name__就是__main__。