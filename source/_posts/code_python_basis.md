---
title: python 学习笔记之基础篇
date: 2022-04-20 21:10:19
categories: Programming
---
# Pref

逢居家懈怠，为缓解焦虑，更好利用空闲时间，启动容易上手的 python3 学习！另，我已有 C 的基础，故一些相同知识点会略过。

# 注释

`#` 注释
`'''` `"""` 多行注释，文档字符串
`#coding=utf-8` 或 `#coding=gbk` 中文注释

# 字符串

字符串连接 `'a' + 'b'`
字符串复制 `'a' * 5`
单双引号均可用，包含时区分开，如 " ' "

print() 返回 None 值
str()

title() 首字母大写
upper() 全大写
lower() 全小写

istitle() 返回布尔值，字符串只包含首字母大写
isupper()
islower()

isalpha() 字符串只包含字母
isalnum() 字符串只包含字母和数字
isdecimal() 字符串只包含数字
isspace() 字符串只包含空格、制表符和换行

startswith()
endswith() 所调用的字符串以该方法传入的字符串开始或结束

join() 参数是一个字符串列表。

```python
' '.join(['My', 'name', 'is', 'Simon'])
'My name is Simon'
```

split() 默认以空格为分隔符拆分字符串，返回一个列表

ljust() 左对齐
rjust()
center() 居中
插入空格来对齐文本；第一个参数是整数长度，第二个可选参数指定一个填充字符。

strip() 删除首尾空白，第二参数可选指定字符删除，字符顺序不重要
lstrip()
rstrip()

```python
spam = 'SpamSpamBaconSpamEggsSpamSpam'
spam.strip('ampS')
'BaconSpamEggs'
```

## u r b f 前缀

u「Unicode」以 Unicode 格式进行编码，常用于中文字符串
r 去掉反斜杠的转义机制，常用于正则表达式，对应 re 模块
b「bytes」bytes 类型字符串
f「format」拼接字符串

```python
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}" # 字符串中插入变量的值
print(full_name)
```

## bytes 和 str 互相转换

```python
str.encode('utf-8')
bytes.decode('utf-8')
```

# 正则表达式 regex

正则表达式的函数都在 re 模块

## 创建对象 re.compile()

传入一个字符串值，表示正则表达式，返回一个 Regex 模式对象。

## 匹配对象 search() group() 方法

Regex 对象的 search()，未找到返回 None；找到返回一个 Match 对象。

Match 对象的 group()，返回被查找字符串中实际匹配的文本。
mo 是一个通用变量名，用于 Match 对象。

```python
phoneNumRegex = re.compile(r'\d{3}-\d{3}-\d{4}')
mo = phoneNumRegex.search('My number is 415-555-4242.')
print('Phone number found: ' + mo.group())
```

## 括号分组

向 group() 传入整数 1 等，取得匹配文本的不同分组；传入 0 或无参数，返回整个匹配文本。
groups() 获取所有分组。

```python
phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d-\d\d\d\d)')
mo = phoneNumRegex.search('My number is 415-555-4242.')
mo.group(1)
'415'

mo.groups()
('415', '555-4242')

areaCode, mainNumber = mo.groups()
print(areaCode)
415

# \( \) 将匹配实际的括号字符
phoneNumRegex = re.compile(r'(\(\d\d\d\)) (\d\d\d-\d\d\d\d)')
```

## 管道匹配多个分组 |

### 返回第一次出现的匹配文本

```python
heroRegex = re.compile (r'Batman|Tina Fey')
mo1 = heroRegex.search('Batman and Tina Fey.')
mo1.group()
'Batman'
```

### 只指定一次前缀

```python
batRegex = re.compile(r'Bat(man|mobile|bat)')
mo = batRegex.search('Batmobile lost a wheel')
mo.group()
'Batmobile'

mo.group(1)
'mobile'
```

## 问号匹配零次或一次

指代字符之前的分组，下同。

```python
batRegex = re.compile(r'Bat(wo)?man')
mo1 = batRegex.search('The Batman')
mo1.group()
'Batman'

mo2 = batRegex.search('The Batwoman')
mo2.group()
'Batwoman'
```

## 星号匹配零次或多次

```python
batRegex = re.compile(r'Bat(wo)*man')
mo3 = batRegex.search('The Batwowowowoman')
mo3.group()
'Batwowowowoman'
```

## 加号匹配一次或多次

```python
batRegex = re.compile(r'Bat(wo)+man')
mo3 = batRegex.search('The Batman')
mo3 == None
True
```

## 花括号匹配特定次数

一个数字，或指定一个范围（最小值，最大值）或不限定最值之一。

(Ha){3} 匹配字符串 'HaHaHa'。

(Ha){3,5} 匹配 'HaHaHa'、'HaHaHaHa'、'HaHaHaHaHa'。

## 贪心和非贪心匹配

Python 的 Regex 默认是贪心的，表示在有二义的情况下，它们会尽可能匹配最长的字符串。
花括号的非贪心版本匹配尽可能最短的字符串，即在结束的花括号后跟着一个问号。

```python
nongreedyHaRegex = re.compile(r'(Ha){3,5}?')
mo = nongreedyHaRegex.search('HaHaHaHaHa')
mo.group()
'HaHaHa
```

## findall() 方法

包含被查找字符串中的所有匹配。不是返回一个 Match 对象。
在正则表达式中没有分组，返回一个字符串列表。
有分组，则返回元组列表。

```python
phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')
phoneNumRegex.findall('Cell: 415-555-9999 Work: 212-555-0000')
['415-555-9999', '212-555-0000']

phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d)-(\d\d\d\d)')
phoneNumRegex.findall('Cell: 415-555-9999 Work: 212-555-0000')
[('415', '555', '1122'), ('212', '555', '0000')]
```

## 字符分类

| 缩写字符 | 表示                                 |
| -------- | ------------------------------------ |
| \d       | 任何一位数字                         |
| \D       | 除 0-9 以外的任何字符                |
| \w       | 任何字母、数字或下划线字符           |
| \W       | 除字母、数字和下划线以外的任何字符   |
| \s       | 空格、制表符或换行符「匹配空白」     |
| \S       | 除空格、制表符和换行符以外的任何字符 |

### 自定义字符分类 []

[a-zA-Z0-9] 匹配所有小写、大写字母和数字。

方括号内，普通的正则表达式符号不会被解释，不需要转义。

### 非字符类 ^

匹配不在这个字符类中的所有字符。

`consonantRegex = re.compile(r'[^aeiouAEIOU]')`

### 开始字符 ^ 结束字符 $

开始处用插入符号 `^`，表明匹配必须发生在被查找文本开始处。
末尾加上美元符号 `$`，表示该字符串必须以这个 Regex 的模式结束。
同时使用，表明整个字符串必须匹配该模式。如 `r'^\d+$'` 匹配从开始到结束都是数字的字符串。

```python
beginsWithHello = re.compile(r'^Hello')
beginsWithHello.search('Hello world!')
<re.Match object; span=(0, 5), match='Hello'>

endsWithNumber = re.compile(r'\d$') # 匹配数字结尾字符
endsWithNumber.search('Your number is 42')
<re.Match object; span=(16, 17), match='2'>
```

### 通配符 . 匹配除换行外所有字符，只匹配一个字符

```python
atRegex = re.compile(r'.at')
atRegex.findall('The cat in the hat sat on the flat mat.')
['cat', 'hat', 'sat', 'lat', 'mat']
```

### .* 匹配除换行外所有字符

```python
nameRegex = re.compile(r'First Name: (.*) Last Name: (.*)')
mo = nameRegex.search('First Name: Alice Last Name: Sweigart')
mo.group(1)
'Alice'
```

`.*` 贪心模式。
`.*?` 非贪心模式。

```python
greedyRegex = re.compile(r'<.*>')
mo = greedyRegex.search('<To serve man> for dinner.>')
mo.group()
'<To serve man> for dinner.>'

nongreedyRegex = re.compile(r'<.*?>')
mo = nongreedyRegex.search('<To serve man> for dinner.>')
mo.group()
'<To serve man>'
```

### re.DOTALL 匹配换行

```python
NewlineRegex = re.compile('.*', re.DOTALL) # 让通配符匹配所有字符，包括换行。
NewlineRegex.search('First\nSecond\nThird').group()
'First\nSecond\nThird'
```

### re.IGNORECASE 不区分大小写匹配

传入 `re.IGNORECASE` 或 `re.I` 作为 `re.compile()` 第二个参数。

### sub() 方法替换字符串

Regex 对象的 sub() 传入两个参数。返回替换后的字符串。
第一个是一个字符串，用于取代发现的匹配；
第二个是要搜索的字符串。

```python
namesRegex = re.compile(r'Agent \w+')
namesRegex.sub('CENSORED', 'Agent Alice gave the documents to Agent Bob.')
'CENSORED gave the documents to CENSORED.'
```

使用匹配的文本本身，作为替换的一部分。sub() 第一个参数中输入 \1、 \2 表示输入分组 1、2 的文本。

字符串中的 \1 将由分组 1 匹配的文本所替代，即 `(\w)` 分组。

```python
agentNamesRegex = re.compile(r'Agent (\w)\w*') # (\w) 匹配 A，\w* 匹配 lice
agentNamesRegex.sub(r'\1****', 'Agent Alice told Agent Carol.')
A**** told C****.
```

## 管理复杂的正则表达式

忽略正则表达式字符串中的空白符和注释。
向 `re.compile()` 传入变量 `re.VERBOSE`，作为第二参数。

难以阅读：

```python
phoneRegex = re.compile(r'((\d{3}|\(\d{3}\))?(\s|-|\.)?\d{3}(\s|-|\.)\d{4}(\s*(ext|x|ext.)\s*\d{2,5})?)')
```

多行书写，加上注释：

```python
phoneRegex = re.compile(r'''(
(\d{3}|\(\d{3}\))? # area code
(\s|-|\.)? # separator
\d{3} # first 3 digits
(\s|-|\.) # separator
\d{4} # last 4 digits
(\s*(ext|x|ext.)\s*\d{2,5})? # extension
)''', re.VERBOSE)
```

```python
someRegexValue = re.compile('foo', re.IGNORECASE | re.DOTALL | re.VERBOSE)
```

# 数

| 数学操作符 | 操作「优先级从高到低」 |
| ---------- | ---------------------- |
| \*\*       | 指数                   |
| %          | 取模                   |
| //         | 整除                   |
| /          | 除                     |
| \*         | 乘                     |
| -          | 减                     |
| +          | 加                     |

int()
float()

## 类假值

0、0.0 和 `' '`（空字符串）被认为是 False

## 大数 - 下划线分组

```python
universe_age = 14_000_000_000
print(universe_age)
14000000000
```

## 多变量赋值

```python
x, y, z = 0, 0, 0
```

## 变量名

1. 只能包含**字母、数字和下划线**
2. 不能以数字开头
3. 区分大小写，常用**小写**

常量，**全大写**表示（无内置常量类型）

# 列表

列表名用**复数**

`[['a','b'],'c']` 可嵌套

## 子列表副本 `[:]`

## 访问最后一个元素 `[-1]`

## 列表连接 `+` 复制 `*`

## 方法

返回值是 None，方法属于单个数据类型，只能在列表上调用。

index() 在列表中查找值

传入一个值，存在于列表中，就返回下标；不在就报 ValueError；有重复只返回第一次出现的下标。

```python
spam = ['hello', 'hi', 'howdy']
spam.index('hello')
```

append() 列表末添加元素，只能一个
insert(0, '') 插入元素

del bicycles[0] 仅删除元素
pop() 删除列表末元素
pop(0) 删除元素，可赋值使用
remove() 根据值删除元素，有重复则只删第一个

sort(reverse=True) 字母顺序排序（逆序）
使用 ASCII 字符顺序；
按照字典顺序排序，`spam.sort(key=str.lower)`，sort() 将列表中所有的表项当成小写。

sorted() 临时字母顺序排序，不改变原列表

> 非所有元素值是小写时，按字母顺序排列列表变复杂。

reverse() 逆序
len() 长度
min()
max()
sum() 数字列表总和

## 可变和不可变数据类型

字符串可看作是**单个文本字符的列表**，列表相关操作也用于字符串。

```python
name = 'Zophie a cat'
newName = name[0:7] + 'the' + name[8:12]
```

## 遍历

```python
cats = ['alice', 'david', 'carolina']
for cat in cats:
    print(cat)
```

## 迭代

```python
spam = ['a', 'b', 'c']
for i in range(len(spam)):
    print(str(i) + ' in spam is ' + spam[i])
```

## 多重赋值技巧

列表的值为多个变量赋值，变量数目和列表长度必须严格相等。

```python
cat = ['fat', 'black', 'loud']
size, color, disposition = cat
```

range(5) 生成数值 0-4
range(1,5) 生成数值 1-4
range(5, -1, -1) 生成数值 5-0

- list() 返回传递值的列表版本
- tuple() 返回传递值元组版本

```python
numbers = list(range(1, 6))
print(numbers)
[1, 2, 3, 4, 5]

list('hello') #可把字符串转成列表
['h', 'e', 'l', 'l', 'o']
```

## 列表解析

```python
# 一般写法
squares = []
for value in range(1, 4):
    squares.append(value**2)
print(squares)

# 列表解析
squares = [value**2 for value in range(1, 4)]
print(squares)
```

## 切片

```python
players = ['charles', 'martina', 'alice']
print(players[0:2]) # [:2] [-2:] [0:3:2]
```

## 复制列表

```python
my_foods = ['pizza', 'falafel', 'cake']
friend_foods = my_foods[:]
```

## 元组 tuple：值不可变的列表，可优化比列表快

```python
dimensions = (50,)
```

> 注：元组是由逗号标识的，圆括号只是方便区分；若定义只包含一个元素的元组，必须在这个元素后面加上逗号。

- 修改元组变量：不能修改元素，可给元组变量重新赋值

```python
dimensions = (200, 50)
for dimension in dimensions:
    print(dimension)
dimensions = (400, 100)
```

# if 语句

在所有算术、比较操作符求值后，依次求值布尔操作符 not，and，or

```python
# 圆括号只是提高可读性
(age_0 >= 21) and (age_1 >= 21)
```

- 检查在列表中是否包含

```python
dogs = ['alice', 'tom']
'tom' in dogs
'jam' not in dogs
```

```python
# 依次判断，满足则执行后退出
age = 12

if age < 4:
    price = 0
elif age < 18:
    price = 25
else: # 可省略
    price = 40

print(f"Price is ${price}.")
```

# 字典

```python
alien = {} # 创建空字典
alien = {'color': 'red'} # 定义字典
aline = {
    'color': 'red',
    'point': '3', # 最后的键值对建议加上逗号
    }

alien['point'] = 5 # 添加键值对
print(alien)
alien['point'] = 3 # 修改字典值
del alien['color'] # 删除必须指定字典名和键

print(speed['fast']) # KeyError
speed_value = alien.get('speed', 'No speed value assigned.') # 用 get() 访问值，避免报错，第一个参数（必选）指定键，第二个参数（可选）返回键不存在时的值，留空默认 None
```

## 遍历所有键值对 items()

```python
for key, value in alien.items(): # items() 返回键值对元组
    print(f"\nkey: {key}")
    print(f"value: {value}")
```

这些数据类型（dict_keys、dict_values、dict_items）可用于 for 循环，不能被修改。

## 遍历所有键 keys()

```python
for key in alien: # 遍历字典，默认遍历所有键
for key in alien.keys(): # keys() 返回键列表
    print(key)
```

## 按特定顺序遍历所有键 sorted()

```python
for key in sorted(alien.key()):
```

## 遍历所有值 values()

```python
for value in alien.values(): # values() 返回值列表
    print(value)
```

## 无重复遍历所有值 set()

```python
for value in set(alien.values()): # 集合 set，用花括号创建集合，用逗号分隔元素
```

> 不同于列表和字典，集合不会以特定的顺序存储元素

## 多重赋值技巧

在 for 循环中将键和值赋给不同的变量

```python
spam = {'color': 'red', 'age': 42}
for k, v in spam.items():
    print('Key: ' + k + ' Value: ' + str(v)) #必须将整型转换成字符型
```

## 某键设置默认值 setdefault()

两参数：检查并为字典中某键设置一个默认值；
该键不存在或没有值时使用它；若存在，方法就会返回键的值，不做修改

# while 循环

input() 接受一个参数做 prompt，提示输入内容，返回字符串

`Ctrl + C` 向程序发送 KeyboardInterrupt 错误，退出无限循环

## 一般循环

```python
prompt = "\nTell me something: "
prompt += "\nEnter 'q' to quit."
msg = ""

while msg != 'q':
    msg = input(prompt)
    if msg != 'q':
        print(msg)
```

## 标志循环

```python
active = True

while active:
    msg = input(prompt)
    if msg == 'q':
        active = False
    else:
        print(msg)
```

## break

```python
while True:
    msg = input(prompt)
    if msg == 'q':
        break
    else:
        print(msg)
```

## continue

```python
n = 0

while n < 10:
    n += 1
    if n % 2 == 0: # 打印奇数
        continue
    print(n)
```

## 在列表间移动元素

```python
names = ['alice', 'brian']
new_names = []

while names:
    temp = names.pop()
    new_names.append(temp)
for new_name in new_names:
    print(new_name)
```

## 删除为特定值的所有列表元素

```python
pets = ['dog', 'cat', 'dog', 'cat', 'rabbit', 'cat']

while 'cat' in pets:
    pets.remove('cat')
print(pets)
```

## 使用用户输入来填充字典

```python
responses = {}

while True:
    name = input("\nname: ")
    response = input("response: ")
    responses[name] = response
    repeat = input("Enter anything to continue or 'n' to quit.")
    if repeat == 'n':
        break
for name, response in responses.items():
    print(f"{name}: {response}")
```

# 函数

形参 parameter
实参 argument

## 实参

```python
def user_age(name, age):
    print(f"{name}: {age}")

user_age('Tom', '18') # 位置实参
user_age(name='Tom', age='18') # 关键字实参
def user_age(name, age='18'): # 默认值，必须先列出无默认值形参

def user_age(name, age, gender=''): # 让实参变可选
    if gender:
        user = f"{name} {age} {gender}"
    else:
        user = f"{name} {age}"
    return user
```

print() 有可选变元 end 和 sep，分别指定在参数末尾打印什么，在参数之间用什么隔开
`print('Hello', end='')` 禁用换行

## 返回字典

```python
# 在条件测试中，None 相当于 False；若包含 age，会添加到字典
def build_person(first_name, last_name, age=None):
    person = {'first': first_name, 'last': last_name}
    if age:
        person['age'] = age
    return person

user = build_person('tom', 'antonio', age=18)
print(user)
```

## 传递列表

```python
def users(names):
    for name in names:
        msg = f"Hello, {name.title()}!"
        print(msg)

usernames = ['hannah', 'ty', 'margot']
users(usernames)
```

## 在函数中修改列表

```python
def change(usernames, new_usernames):
    while usernames:
        temp = usernames.pop()
        new_usernames.append(temp)

def show(new_usernames):
    for new_username in new_usernames:
        print(new_username)

usernames = ['hannah', 'ty', 'margot']
new_usernames = []

change(usernames, new_usernames)
show(new_usernames)
```

## 禁止函数修改列表，[:] 创建列表副本

```python
change(usernames[:], new_usernames)
```

## 传递任意数量的实参

```python
# 星号创建一个 toppings 的空元组，该形参必须放在最后
def make_pizza(size, *toppings):
    print(f"size: {size}")
    for topping in toppings:
        print(f"{topping}")

make_pizza(6, 'pepperoni')
make_pizza(8, 'mushrooms', 'green peppers')
```

## 使用任意数量的关键字实参

```python
# 双星号创建一个 info 的空字典，该形参必须放在最后
def build_profile(first, last, **info):
    info['first_name'] = first
    info['last_name'] = last
    return info

profile = build_profile('albert', 'einstein', location='princeton', field='physics')
print(profile)
```

## 函数模块化及别名

```python
'''pizza.py'''
def make_pizza(size, *toppings):
    print(f"size: {size}")
    for topping in toppings:
        print(f"{topping}")
def show():
    print("...")


'''main.py'''
import pizza

pizza.make_pizza(6, 'pepperoni')
pizza.make_pizza(8, 'mushrooms', 'green peppers')

from pizza import make_pizza, show # 导入模块中特定函数

make_pizza(6, 'pepperoni')

from pizza import * # 星号导入模块所有函数，可能与当前程序的函数覆盖，不建议用

import pizza as p # 指定模块别名
from pizza import make_pizza as mp # 指定函数别名
```

## 作用域

- 全局作用域中代码不能使用局部变量
- 局部作用域可访问全局变量
- 不同的作用域中，可以用相同的名字命名不同的变量

- global 语句强制函数中的一个变量引用该全局变量

# 类

```python
'''
类中函数称为方法，规定双下划线；
调用 __init__ 方法创建 Car 实例，自动传入实参 self，
必须有一个参数位于最前面，
方法是一个指向实例本身的引用，让实例能够访问类中的属性和方法；
Car 类创建实例时，只需给其他形参提供值；
前缀为 self 的变量可供类中所有方法使用，可通过任何实例访问；
类看作是如何创建实例的说明。
'''
class Car:
    def __init__(self, make, model, year):
        self.make = make # 可通过实例访问的变量称为属性
        self.model = model
        self.year = year
        self.odometer = 0 # 属性默认值

    def info(self):
        name = f"{self.year} {self.make} {self.model}"
        return name.title()

    def read_odometer(self):
        print(f"{self.odometer}")

    def update_odometer(self, mileage):
        # 禁止里程表读数往回调
        if mileage >= self.odometer:
            self.odometer = mileage
        else:
            print("Don't roll back an odometer.")

    def increment_odometer(self, miles):
        self.odometer += miles

car = Car('audi', 'a4', 2019) # 根据类创建实例

print(f"{car.make}") # 访问属性

print(car.info())
car.read_odometer() # 调用方法

car.odometer = 20 # 直接修改属性值
car.update_odometer(35) # 通过方法修改属性值
car.increment_odometer(10) # 通过方法递增属性值
```

## 继承

```python
'''
创建子类时，父类必须包含在当前文件中，且位于子类前；
父类也称超类（superclass）
'''
class Car:
    --snip--

class EletricCar(Car): # 子类继承父类
    def __init__(self, make, model, year): # 初始化父类属性
        super().__init__(make, model, year) # super() 函数可调用父类方法
        self.battery = 100 # 新属性，父类不包含

    def info_battery(self):
        print(f"{self.battery}")

    # 重写父类方法，符合子类的需求

el_car = EletricCar('tesla', 'model s', 2019)
print(el_car.info())
```

```python
class Car:
    --snip--

class Battery:
    def __init__(self, battery=100):
        self.battery = battery

    def info_battery(self):
        print(f"{self.battery}")

    def get_range(self): # 续航里程
        if self.battery == 100:
            range = 315
        elif self.battery == 75:
            range = 260
        print(f"{range}")

class EletricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery = Battery() # 将实例用作属性

el_car = EletricCar('tesla', 'model s', 2019)
print(el_car.info())
# 在实例 el_car 中查找属性 battery，并对存储在该属性中的 Battery 实例调用方法 info_battery()
el_car.battery.info_battery()
```

## 导入类

```python
'''car.py'''
class Car:
    --snip--

class Battery:
    --snip--

class EletricCar(Car):
    --snip--
```

```python
'''my_car.py'''
# 最好是导入整个模块
from car import Car, EletricCar # 导入子类

my_car = Car('audi', 'a4', 2019)
print(my_car.info())
my_car.odometer = 23
my_car.read_odometer()
```

# 文件

## 读取整个文件 - read()

```python
'''
函数 open() 返回一个表示文件的对象，
赋给 file_object（只在 with 内可用），用方法 read() 读取；
关键字 with 在不再需要访问文件后将其关闭；
close() 关闭文件可能存在各种问题；
read() 到达文件尾时返回一个空字符串，表现为空行；
文本文件内容都被解读为字符串。
'''
with open('demo.txt') as file_object:
    contents = file_object.read()
print(contents.rstrip()) # 删除多余空行
```

## 逐行读取 - for 循环

```python
'''
输出可能每行中间有一个空行，
print() 调用有一个换行符，文件本身有一个换行符。
'''
filename = 'demo.txt'

with open(filename) as file_object:
    for line in file_object:
        print(line.rstrip())
```

## 创建包含文件各行内容的列表 - readlines()

```python
filename = 'demo.txt'

with open(filename) as file_object:
    lines = file_object.readlines() # 读取文件每一行，并存储在列表中
for line in lines:
    print(line.rstrip())
```

## 使用文件内容 - 空字符串

```python
filename = 'demo.txt'

with open(filename) as file_object:
    lines = file_object.readlines()

use = ''
for line in lines:
    use += line.rstrip()

print(use)
```

## 写入文件 - write() - rwar+w

```python
'''
读取模式：r 读、w 写、a 附加、r+w 读写；
省略模式实参，默认只读模式打开文件；
写入文件不存在则自动创建，存在则覆盖重写；
只能将字符串写入文本文件。
'''
filename = 'demo.txt'

with open(filename, 'w') as file_object:
    file_object.write('anything')

with open(filename, 'a') as file_object:
    file_object.write('\naddition')
```

> 注：Windows 使用 \ 显示文件路径，但在代码中用 /
绝对路径较长，赋给一个变量使用；
python 可处理的数据量没有限制，只要系统内存足够。

# 异常处理

## ZeroDivisionError 异常

```python
'''
python 用特殊对象「异常」来管理程序执行期间发生的错误；
避免用户看到 traceback
'''
def spam(divideBy):
    try:
        return 42 / divideBy
    except ZeroDivisionError:
       print("You can't divide by zero!")

print(spam(0))
```

## FileNotFoundError 异常

```python
def count_words(filename):
    try:
        # 一般用 f 表示文件对象；
        # 系统默认编码与读取文件编码不一致时，需指定编码
        with open(filename, encoding='utf-8') as f:
            contents = f.read()
    except FileNotFoundError:
        print(f"Sorry, the file {filename} does not exist.")
    '''
    except FileNotFoundError:
        pass # 静默失败
    '''
    # pass 语句还充当占位符，提醒你在程序的某处无操作，也许以后要做什么。

    else:
        # 分析文本
        words = contents.split()
        num_words = len(words)
        print(f"{filename} has about {num_words} words.")

filename = 'alice.txt' # 处理单文件
count_words(filename)

filenames = ['alice.txt', 'siddhartha.txt'] # 处理多文件
for filename in filenames:
    count_words(filename)
```

# 存储数据 - json

JSON（JavaScript Object Notation）

## dump() load()

```python
import json

numbers = [2, 3, 5, 7, 11, 13]
filename = 'numbers.json'

with open(filename, 'w') as f:
    # 该函数接受两个实参：存储的数据、文件对象
    json.dump(numbers, f)

with open(filename) as f:
    numbers = json.load(f) # 加载信息

print(numbers)
```

## 保存和读取用户生成的数据

```python
# 如果以前存储了用户名，就加载它。
# 否则，提示用户输入用户名并存储它。
import json

filename = 'username.json'

try:
    with open(filename) as f:
        username = json.load(f)
except FileNotFoundError:
    username = input("What is your name? ")
    with open(filename, 'w') as f:
        json.dump(username, f)
        print(f"We'll remember you when you come back, {username}!")
else:
    print(f"Welcome back, {username}!")
```

# 重构

```python
'''
代码能正确运行，但将其划分为便于改进的一系列完成具体工作的函数，该过程称为重构。
'''
import json

def greet_user():
    """问候用户，并指出其名字。"""
    filename = 'username.json'
    try:
        with open(filename) as f:
            username = json.load(f)
    except FileNotFoundError:
        username = input("What is your name? ")
        with open(filename, 'w') as f:
            json.dump(username, f)
            print(f"We'll remember you when you come back, {username}!")
    else:
        print(f"Welcome back, {username}!")

greet_user()
```

## 重构 greet_user()

```python
import json

def get_stored_username():
    filename = 'username.json'
    try:
        with open(filename) as f:
            username = json.load(f)
    except FileNotFoundError:
        return None
    else:
        return username

def get_new_username():
    username = input("What is your name? ")
    filename = 'username.json'
    with open(filename, 'w') as f:
        json.dump(username, f)
    return username

def greet_user():
    username = get_stored_username()
    if username:
        print(f"Welcome back, {username}!")
    else:
        username = get_new_username()
        print(f"We'll remember you when you come back, {username}!")

greet_user()
```

# 测试

## 模块函数的定义和调用

```python
'''name_function.py'''
def get_formatted_name(first, last):
    full_name = f"{first} {last}"
    return full_name.title()
```

```python
'''names.py'''
import name_function

print("Enter 'q' at any time to quit.")

while True:
    first = input("\nfirst name: ")
    if first == 'q':
        break
    last = input("last name: ")
    if last == 'q':
        break
    formatted_name = get_formatted_name(first, last)
    print(f"\tNeatly formatted name: {formatted_name}.")
```

## 单元测试和测试用例 - unittest

```python
'''
运行 test_name_function.py 时，
所有以 test_ 打头的方法都将自动运行。
'''
'''test_name_function.py'''
import unittest
from name_function import get_formatted_name

class NamesTestCase(unittest.TestCase):
    def test_first_last_name(self):
        """能够正确地处理像 Janis Joplin 这样的姓名吗？"""
        formatted_name = get_formatted_name('janis', 'joplin')
        # 断言方法：核实得到的结果是否与期望一致
        self.assertEqual(formatted_name, 'Janis Joplin')

# 特殊变量 __name__，该变量在程序执行时设置
# 若该文件作为主程序执行，__name__ 将被设置为 __main__
if __name__ == '__main__':
    unittest.main() # 调用它来运行测试用例

'''
运行测试文件，输出如下：
第一行句点表明有一个测试通过了；
OK 表明该测试用例中的所有单元测试都通过了。
'''
.
----------------------
Ran 1 test in 0.001s
OK
```

## 未通过的测试

```python
'''
使函数能处理中间名，但无法正确处理只有名和姓的情况
'''
'''name_function.py'''
def get_formatted_name(first, middle, last):
    full_name = f"{first} {middle} {last}"
    return full_name.title()

'''
运行测试文件，输出如下：
第一行字母 E，指出有一个单元测试导致了错误；
第三行指出 NamesTestCase 中的 test_first_last_name() 导致了错误；
Traceback 指出函数调用 get_formatted_name(...) 有问题；
最后指出整个测试用例未通过，因为发生了一个错误。
'''
E
================================
ERROR: test_first_last_name (__main__.NamesTestCase)
--------------------------------
Traceback (most recent call last):
  File "test_name_function.py", line 8, in test_first_last_name
    formatted_name = get_formatted_name('janis', 'joplin')
TypeError: get_formatted_name() missing 1 required positional argument: 'last'
--------------------------------
Ran 1 test in 0.001s
FAILED (errors=1)
```

## 处理未通过的测试

```python
'''name_function.py'''
def get_formatted_name(first, last, middle=''):
    if middle:
        full_name = f"{first} {middle} {last}"
    else:
        full_name = f"{first} {last}"
    return full_name.title()
```

## 添加新测试

```python
'''
用于测试包含中间名的姓名
'''
'''test_name_function.py'''
--snip--
class NamesTestCase(unittest.TestCase):
    def test_first_last_name(self):
        --snip--

    def test_first_last_middle_name(self):
    formatted_name = get_formatted_name('wolf', 'mozart', 'amadeus')
    self.assertEqual(formatted_name, 'Wolf Amadeus Mozart')

if __name__ == '__main__':
    unittest.main()

'''
第一行句点表明有两个测试通过了……
'''
..
----------------------
Ran 2 test in 0.001s
OK
```

## 断言方法

unittest 模块中的断言方法，只能在继承 unittest.TestCase 的类中使用这些方法

| 方法                    | 用途                 |
| ----------------------- | -------------------- |
| assertEqual(a, b)       | 核实 a == b          |
| assertNotEqual(a, b)    | 核实 a != b          |
| assertTrue(x)           | 核实 x 为 True       |
| assertFalse(x)          | 核实 x 为 False      |
| assertIn(item, list)    | 核实 item 在 list 中 |
| assertNotIn(item, list) | 核实 item 不在 list  |

## 模块类定义与调用

```python
'''survey.py'''
class AnonymousSurvey:
    """收集匿名调查问卷的答案。"""
    def __init__(self, question):
        self.question = question
        self.responses = []

    def show_question(self):
        print(self.question)

    def store_response(self, new_response):
        self.responses.append(new_response)

    def show_results(self):
        print("Survey results:")
        for response in self.responses:
            print(f"- {response}")
```

```python
'''language_survey.py'''
from survey import AnonymousSurvey

question = "What language did you first learn to speak?"
my_survey = AnonymousSurvey(question)

my_survey.show_question()
print("Enter 'q' at any time to quit.")

while True:
    response = input("Language: ")
    if response == 'q':
        break
    my_survey.store_response(response)

my_survey.show_results()
```

## 测试类

```python
'''test_survey.py'''
import unittest
from survey import AnonymousSurvey

class TestAnonymousSurvey(unittest.TestCase):
    def test_store_single_response(self):
        """测试单个答案会被妥善存储。"""
        question = "What language did you first learn to speak?"
        my_survey = AnonymousSurvey(question)
        my_survey.store_response('English')
        self.assertIn('English', my_survey.responses)

    def test_store_three_responses(self):
        question = "What language did you first learn to speak?"
        my_survey = AnonymousSurvey(question)
        responses = ['English', 'Spanish', 'Mandarin']
        for response in responses:
            my_survey.store_response(response)
        for response in responses:
            self.assertIn(response, my_survey.responses)

if __name__ == '__main__':
    unittest_main()
```

## 方法 setUp()

```python
# 若 TestCase 类中包含 setUp()，python 将先运行它，再运行以 test_ 打头的方法。
'''test_survey.py'''
import unittest
from survey import AnonymousSurvey

class TestAnonymousSurvey(unittest.TestCase):
    def setUp(self):
        """创建一个调查对象和一组答案，供使用的测试方法使用。"""
        question = "What language did you first learn to speak?"
        self.my_survey = AnonymousSurvey(question)
        self.responses = ['English', 'Spanish', 'Mandarin']

    def test_store_single_response(self):
        self.my_survey.store_response(self.responses[0])
        self.assertIn(self.responses[0], self.my_survey.responses)

    def test_store_three_responses(self):
        for response in self.responses:
            self.my_survey.store_response(response)
        for response in self.responses:
            self.assertIn(response, self.my_survey.responses)

if __name__ == '__main__':
    unittest_main()
```

> 注：运行测试用例时，每完成一个单元测试，都打印一个字符：
通过时打印一个句点，报错时打印一个 E，
而测试导致断言失败时则打印一个 F。



# 关键字

| False  | await    | else    | import   | pass   |
| :----- | :------- | :------ | :------- | :----- |
| None   | break    | except  | in       | raise  |
| True   | class    | finally | is       | return |
| and    | continue | for     | lambda   | try    |
| as     | def      | from    | nonlocal | while  |
| assert | del      | global  | not      | with   |
| async  | elif     | if      | or       | yield  |

# 内置函数

| abs()         | delattr()   | hash()       | memoryview() | set()     |
| :------------ | :---------- | :----------- | :----------- | :-------- |
| all()         | dict()      | help()       | min()        | setattr() |
| any()         | dir()       | hex()        | next()       | slice()   |
| ascii()       | divmod()    | id()         | object()     | sorted()  |
| bin()         | enumerate() | input()      | oct()        | staticme  |
| bool()        | eval()      | int()        | open()       | str()     |
| breakpoint()  | exec()      | isinstance() | ord()        | sum()     |
| bytearray()   | filter()    | issubclass() | pow()        | super()   |
| bytes()       | float()     | iter()       | print()      | tuple()   |
| callable()    | format()    | len()        | property()   | type()    |
| chr()         | frozenset() | list()       | range()      | vars()    |
| classmethod() | getattr()   | locals()     | repr()       | zip()     |
| compile()     | globals()   | map()        | reversed()   | __import  |
| complex()     | hasattr()   | max()        | round()      |

# Git 版本控制

## 1. 创建文件夹 git_practice

## 2. 创建程序 `hello_git.py`

```python
print("Hello Git!")
```

## 3. 忽略文件 .gitignore

```git
__pycache__
```

> git bash: `touch .gitignore`
dos: `ren n.txt .gitignore`

## 4. 初始化仓库 `git init`

`Initialized empty Git repository in git_practice/.git/`

> Git 用来管理仓库的文件都存储在 .git 中，不用管它但不能删除。

## 5. 检查状态 `git status`

```git
On branch main
No commits yet
Untracked files: # 指出未被跟踪的文件
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        hello_git.py
# 当前无提交，但指出了可能需要加入仓库的未跟踪文件
nothing added to commit but untracked files present (use "git add" to track)
```

## 6. 加入仓库 `git add .`

项目中未被跟踪的文件都加入仓库中，但未提交

```git
On branch main
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitignore
        new file:   hello_git.py
```

## 7. 执行提交 `git commit -m "Started project"`

标志 -m 让后面信息记录到项目的历史记录中。

```git
[main (root-commit) a3f6a37] Started project
 2 files changed, 2 insertion(+)
 create mode 100644 .gitignore
 create mode 100644 hello_git.py

On branch master
nothing to commit, working tree clean
# 工作树是干净的，若非如此，可能提交前忘记了添加文件。
```

## 8. 查看提交历史 `git log`

```git
commit b4a4af78247b33c27de89b57d4dde2369f712ab4 (HEAD -> main)
Author: xxx <xxx@gmail.com>
Date:   Mon Apr 24 16:34:50 2023 +0800
    Started project
```

每次提交 Git 都会生成唯一的引用 ID，长 40 字符。
记录提交人，提交时间，及提交时的指定信息。

精简版：`git log --pretty=oneline`

```git
b4a4af78247b33c27de89b57d4dde2369f712ab4 (HEAD -> main) Started project
```

## 9. 第二次提交 `git commit -am "Extended greeting"`

标志 -a 让仓库中所有修改了的文件都加入当前提交中

```python
'''hello_git.py'''
print("Hello Git!")
print("Hello everyone.")
```

```git
# 检查状态，指出所做修改未提交
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello_git.py
no changes added to commit (use "git add" and/or "git commit -a")

[main 35530cb] Entended greeting
 1 file changed, 1 insertion(+)

# 查看状态和记录，同上
```

## 10. 撤销修改 `git checkout .`

```git
Updated 1 path from the index

# 查看状态，输出工作树干净，文件中最后一行代码被删除。
```

git checkout 能恢复到以前的任意提交。
git checkout . 放弃最后一次**提交后**所做的修改，恢复到**最后一次提交**的状态。

## 11. 检出以前的提交 `git checkout b4a4af`

末尾指定该提交的引用 ID 前 6 字符。

```git
Note: switching to 'b4a4af'...
HEAD is now at b4a4af7 Started project

git checkout main

Previous HEAD position was b4a4af7 Started project
Switched to branch 'main'
```

检出以前的提交后，将离开分支 main，进入分离头指针（detached HEAD）状态。
HEAD 指针表示当前提交的项目状态。

## 12. 重置到以前的提交 `git reset --hard b4a4af`

## 13. 删除仓库 `rm -rf .git`

当仓库的历史记录弄乱了，无法恢复时，个人可删除历史记录，再重建仓库，不影响文件当前状态。

可直接删除 .git 或 cmd 下：

```cmd
rmdir /s .git
```

# 缩进规则的例外

> 在源码文件中，列表可以跨越几行，直到结束方括号。
行末使用续行字符 `\`，将一条指令写成多行。

# 标准库

## random

```python
# 创建与安全相关的应用程序时，勿用 random
from random import randint, choice

# 随机返回两个整数之间（含）的整数
randint(1, 6)

# 列表/元组为参数，随机返回其中一个元素
players = ['charles', 'michael', 'florence', 'eli']
choice(players)
```



# 第三方模块

## 自动排版

```python
pip install yapf
```



# Refer

《Python编程快速上手——让繁琐工作自动化第一版》
《python编程从入门到实践第二版》
[git - Windows下创建 .gitignore 文件](https://blog.csdn.net/972301/article/details/49586273)
[Python 字符串前面加u,r,b,f的含义](https://blog.csdn.net/sinat_38682860/article/details/108848994)
[python格式化代码【自动排版】 yapf、black风格选择](https://blog.csdn.net/sinat_28442665/article/details/118901285)

# PS

啃过 C/C++ 的选手，再学 python 进展之快着实超乎我的意料，晃荡大学四年，编程语言这方面总算开始有真正的基础了，汗颜啊！