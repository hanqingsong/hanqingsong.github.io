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

## 变量
    
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

## 遍历
```
for  item in  list:
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

## 字典
字典是一系列键值对，用放在花括号{}中的一系列键—值对表示
删除元素 del userMap['name']



## 函数
    
## 帮助文档
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