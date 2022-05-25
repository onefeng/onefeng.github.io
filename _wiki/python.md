---
layout: wiki
title: python
categories: python
description: python 基础
keywords: python
---

python基础

## 基础语法

- 标准数据类型

- 条件判断

- 循环

- 函数

- 类

## re正则模块

### 匹配规则


| 模式      | 描述                                               |
|---------|--------------------------------------------------|
| \\w     | 匹配字母、数字及下划线                                      |
| \\W     | 匹配不是字母、数字及下划线的字符                                 |
| \\s     | 匹配任意空白字符，等价于\[\\t\\n\\r\\f\]                     |
| \\S     | 匹配任意非空白字符                                        |
| \\d     | 匹配任意数字，等价于\[0\-9\]                               |
| \\D     | 匹配任意非数字的字符                                       |
| \\A     | 匹配字符串开头                                          |
| \\Z     | 匹配字符串结尾，如果存在换行，只匹配到换行前的结束字符串                     |
| \\z     | 匹配字符串结尾，如果存在换行，同时还会匹配换行符                         |
| \\G     | 匹配最后完成匹配时的位置                                     |
| \\n     | 匹配一个换行符                                          |
| \\t     | 匹配一个制表符                                          |
| ^       | 匹配一行字符串的开头                                       |
| $       | 匹配一行字符串的结尾                                       |
| \.      | 匹配任意字符，除了换行符，当\.re\.DOTALL标记被指定时，则可以匹配包括换行符的任意字符 |
| \[…\]   | 用来表示一组字符，单独列出，比如\[amk\]匹配a、m或k                   |
| \[^…\]  | 不在\[\]中的字符，比如\[^abc\]匹配除了a、b、c之外的字符              |
| \*      | 匹配0个或多个表达式                                       |
| \+      | 匹配1个或多个表达式                                       |
| ?       | 匹配0个或1个前面的正则表达式定义的片段，非贪婪方式                       |
| \{n\}   | 精准匹配n个前面的表达式                                     |
| \{n,m\} | 精准匹配n到m次由前面正则表达式定义的片段，贪婪方式                       |
| a\|b    | 匹配a或b                                            |
| \(\)    | 匹配括号内的表达式，也表示一个组                                 |

### match

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回None。re.match(pattern, string, flags=0)

```python
import re
 
line = "Cats are smarter than dogs"
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I) #re.M多行匹配模式 re.I 忽略大小写 ()表示分组
print(matchObj.group()) # 全匹配
print(matchObj.group(1)) # 返回第一个分组
print(matchObj.group(2))  # 返回第一个分组
print(matchObj.groups())  # 返回所有分组
```

输出
```
Cats are smarter than dogs
Cats
smarter
('Cats', 'smarter')
```

### search

re.search 扫描整个字符串并返回第一个成功的匹配。re.search(pattern, string, flags=0)

```python
import re
 
line = "Cats are smarter than dogs"
 
matchObj = re.match( r'dogs', line, re.M|re.I)
if matchObj:
   print("match --> matchObj.group() : ", matchObj.group())
else:
   print("No match!!")
 
matchObj = re.search( r'dogs', line, re.M|re.I)
if matchObj:
   print("search --> searchObj.group() : ", matchObj.group())
else:
   print("No match!!")
```

输出
```shell
No match!!
search --> searchObj.group() :  dogs
```

```python
import re
s = '1102231990xxxxxxxx'
res = re.search(r'(?P<province>\d{3})(?P<city>\d{3})(?P<born_year>\d{4})',s)
print(res.groupdict())
```

输出
```python
{'province': '110', 'city': '223', 'born_year': '1990'}
```
### compile

re.compile用于预编译正则表达式对象，以便在后面的匹配中复用。语法格式 re.compile(pattern[, flags])

```python
import re
pattern=re.compile(r'd{2}',re.S)
```

### findall

re.findall在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。

```python
import re
pattern = re.compile(r'\d+',re.S)
result = re.findall(pattern,'runoob 123 google 456')
print(result)

```

输出
```python
['123', '456']
```

### sub

re.sub用于替换修改文本

```python
import re
content='988sudhfi8893'
content=re.sub('\d+','',content)
print(content)
```

输出
```python
sudhfi
```