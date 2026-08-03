---
title: Python 基础语法笔记
date: 2026-08-03 22:45:00
categories:
  - 技能
  - 基础技能
  - Python
tags: [Python, 基础语法, 笔记]
---
# Python 基础语法笔记

> 以代码为主的基础语法笔记，覆盖：基础语法、变量、运算符、输入输出、条件判断、循环、字符串。
> 重点在"怎么写"。注释即说明，看代码即可。

---

## 目录

1. [基础语法](#1-基础语法)
2. [变量与数据类型](#2-变量与数据类型)
3. [运算符](#3-运算符)
4. [输入输出](#4-输入输出)
5. [条件判断](#5-条件判断)
6. [循环](#6-循环)
7. [字符串](#7-字符串)

---

## 1. 基础语法

```python
# 注释：单行
"""
多行注释
用三引号
"""

# 缩进表示代码块（4个空格）
# 语句末尾不需要分号

# 一行写多条语句用分号
a = 1; b = 2; c = 3

# 一行太长用反斜杠续行
total = 1 + 2 + 3 + \
        4 + 5 + 6

# 或用括号自动续行
nums = [1, 2, 3,
        4, 5, 6]

# 打印
print("Hello")
print("a", "b", "c")              # a b c（默认空格分隔）
print("a", "b", sep="-")          # a-b（自定义分隔符）
print("Hello", end="")            # 不换行
```

---

## 2. 变量与数据类型

### 2.1 标识符命名规范

```python
# 规则：只能由 字母 数字 下划线 组成
#       不能以数字开头
#       不能是关键字（if for class def ...）
#       区分大小写（name 和 Name 是不同变量）
#       不能含空格和特殊字符（@ $ % 等）
#       见名知意，避免 a b c 这类无意义名字

# 合法命名
name = "张三"
user_name = "李四"
_name = "王五"
userName2 = "赵六"
NAME = "常量风格"

# 非法命名（会报错）
# 2name = "x"      # 数字开头
# user-name = "x"  # 含连字符
# user name = "x"  # 含空格
# class = "x"      # 关键字

# 查看所有关键字
import keyword
print(keyword.kwlist)

# 命名风格约定（PEP 8）
user_age = 20          # 变量/函数：snake_case 小写下划线
MAX_SIZE = 100         # 常量：UPPER_CASE 全大写
class Student: pass    # 类名：PascalCase 大驼峰
_user = "私有"          # 单下划线开头：约定私有
__user = "改名私有"     # 双下划线开头：触发改名
__init__ = "魔法方法"   # 双下划线包围：系统定义，不要自创
```

### 2.2 变量的赋值

```python
# 赋值即创建，无需声明类型
name = "张三"
age = 18
height = 1.75
is_ok = True
nothing = None

# 同一标识符重复赋值，最终值是最后一次赋的
x = 1
x = 2
x = 3
print(x)        # 3

# 类型也可以变（动态类型）
x = 10          # int
x = "hello"     # str
x = [1, 2]      # list
print(x)        # [1, 2]
print(type(x))  # <class 'list'>

# 多变量赋值
a, b, c = 1, 2, 3
x = y = z = 0

# 交换变量
a, b = b, a
```

### 2.3 类型查看与判断

```python
# 查看类型
print(type(age))         # <class 'int'>

# isinstance(obj, 类型)：判断对象是否属于某类型，返回 bool
isinstance(10, int)        # True
isinstance("abc", str)     # True

# 支持类型元组（满足其一即可）
isinstance(10, (int, float))   # True

# isinstance 认继承链，type() 不认（核心区别）
class Animal: pass
class Dog(Animal): pass
d = Dog()
isinstance(d, Animal)   # True（子类实例也算父类）
type(d) == Animal       # False（type 只认精确类型）

# bool 是 int 子类，所以：
isinstance(True, int)   # True
type(True) == int       # False

# 判断 None 推荐用 is
x is None
```

### 2.4 类型转换

```python
int("123")        # -> 123
float("3.14")     # -> 3.14
str(100)          # -> "100"
bool(0)           # -> False（0/空/None 为 False，其余为 True）
list("abc")       # -> ['a', 'b', 'c']
```

### 2.5 bool 本质是 int 的子类

```python
issubclass(bool, int)     # True
type(True)                # <class 'bool'>

# 数值运算时 True 当 1，False 当 0
True + 1                  # 2
True + True               # 2
False + 0                 # 0
True * 5                  # 5
False * 10                # 0
True - False              # 1

# 求列表中 True 的个数
sum([True, False, True, True])   # 3

# 比较运算结果可直接参与运算
sum([x > 5 for x in [3, 6, 8, 2]])   # 2（大于5的有2个）

# bool 可以当 int 用
[1, 2, 3][True]           # 2（索引1）
{True: "是", False: "否"}  # 等价于 {1: "是", 0: "否"}

# 但注意：True == 1，True is not 1
True == 1                 # True
True is 1                 # False（不同对象）
```

---

## 3. 运算符

```python
# 算术
7 + 2     # 9    加
7 - 2     # 5    减
7 * 2     # 14   乘
7 / 2     # 3.5  除（浮点）
7 // 2    # 3    整除
7 % 2     # 1    取余
7 ** 2    # 49   幂

# 比较（返回 bool）
3 > 2      # True
3 < 2      # False
3 >= 3     # True
3 <= 2     # False
3 == 3     # True   相等（两个等号）
3 != 2     # True   不等

# 逻辑
True and False    # False
True or False     # True
not True          # False

# 复合赋值
x = 10
x += 5    # x = x + 5
x -= 3
x *= 2
x /= 4
x //= 2
x %= 3
x **= 2

# 成员运算
3 in [1, 2, 3]        # True
"a" in "abc"          # True
5 not in [1, 2, 3]    # True

# 身份运算（判断是否同一对象）
a is None
a is not None
```

---

## 4. 输入输出

```python
# input 返回字符串
name = input("姓名：")
age = int(input("年龄："))     # 转整数
height = float(input("身高：")) # 转浮点

# print 格式化
name = "张三"
age = 20
price = 9.5

# f-string（推荐）
print(f"我叫{name}，今年{age}岁")
print(f"{price:.2f}")          # 保留2位小数 -> 9.50
print(f"{3.14159:.3f}")        # 3.142
print(f"{255:x}")              # ff（十六进制）
print(f"{12345:,}")            # 12,345（千分位）

# format 方法
"{}今年{}".format(name, age)
"{1}在前{0}在后".format("A", "B")    # B在前A在后
"{name}的年龄是{age}".format(name="李四", age=25)

# 百分号（旧式）
"我叫%s，今年%d岁" % (name, age)
"%.2f" % 3.14159    # 3.14

# 常用对齐
print(f"{'左':<10}|")    # 左对齐
print(f"{'中':^10}|")    # 居中
print(f"{'右':>10}|")    # 右对齐
print(f"{42:0>5}")       # 00042（补零）
```

---

## 5. 条件判断

### 5.1 if / else / elif

```python
# if
if age >= 18:
    print("成年")

# if-else
if score >= 60:
    print("及格")
else:
    print("不及格")

# if-elif-else
if score >= 90:
    print("优秀")
elif score >= 80:
    print("良好")
elif score >= 60:
    print("及格")
else:
    print("不及格")

# 嵌套
if age >= 18:
    if has_ticket:
        print("可入场")
    else:
        print("请购票")

# 多条件
if 18 <= age <= 60:     # 链式比较
    print("工作年龄")

if age > 0 and age < 150:
    pass

# 空值判断
if x:        # x 非0/非空/非None 为 True
    pass
if not x:    # x 为0/空/None 时 True
    pass
```

### 5.2 三元表达式

```python
status = "成年" if age >= 18 else "未成年"
```

### 5.3 match...case

```python
# 基本语法
status = 404
match status:
    case 200:
        print("OK")
    case 404:
        print("Not Found")
    case 500:
        print("Error")
    case _:                  # 默认，相当于 else
        print("Unknown")

# 多值匹配用 |
match color:
    case "red" | "blue" | "green":
        print("原色")
    case _:
        print("其他")

# 解构匹配（自动捕获变量）
point = (3, 4)
match point:
    case (0, 0):
        print("原点")
    case (x, 0):
        print(f"x轴 x={x}")
    case (x, y):
        print(f"({x},{y})")

# 守卫 guard：case 后加 if 条件
match n:
    case x if x > 0:
        print("正数")
    case 0:
        print("零")
    case _:
        print("负数")
```

---

## 6. 循环

### 6.1 while 循环

```python
i = 1
while i <= 5:
    print(i)
    i += 1

# while-else（正常结束才执行 else）
n = 0
while n < 3:
    print(n)
    n += 1
else:
    print("结束")

# 案例：求 1~100 偶数和（while + if + else）
i = 1
sum = 0
while i <= 100:
    if i % 2 == 0:       # 偶数才累加
        sum = sum + i
    i = i + 1
else:
    print(f"1到100之间的偶数和{sum}")   # 1到100之间的偶数和2550
```

### 6.2 for 循环

```python
# 遍历
for c in "Python":
    print(c)

for item in [1, 2, 3]:
    print(item)

for k, v in {"a": 1, "b": 2}.items():
    print(k, v)

# range
range(5)         # 0 1 2 3 4
range(1, 6)      # 1 2 3 4 5
range(0, 10, 2)  # 0 2 4 6 8（步长）
range(10, 0, -1) # 10 9 8 ... 1（倒序）

for i in range(1, 10):
    print(i)
```

### 6.3 break / continue / pass

```python
# break 跳出整个循环
for i in range(10):
    if i == 5:
        break
    print(i)      # 0 1 2 3 4

# continue 跳过本次
for i in range(5):
    if i == 2:
        continue
    print(i)      # 0 1 3 4

# pass 占位（空语句）
for i in range(5):
    pass
```

### 6.4 嵌套循环：九九乘法表

```python
for i in range(1, 10):
    for j in range(1, i + 1):
        print(f"{j}*{i}={i*j}", end="\t")
    print()
# 输出示例（第一行）：1*1=1
#               （第二行）：1*2=2	2*2=4

# end="\t" 的作用：每个算式后用制表符结尾，横向对齐且不换行
#                （默认 end="\n" 会换行，这里改成 \t 实现同行输出）
# print()  的作用：空打印，只输出一个换行符
#                内层循环结束一行后，用 print() 换到下一行
```

### 6.5 推导式（重点）

```python
# 列表推导式
nums = [i for i in range(10)]              # [0,1,...,9]
squares = [i**2 for i in range(1, 6)]      # [1,4,9,16,25]
evens = [i for i in range(10) if i % 2 == 0]  # [0,2,4,6,8]
pairs = [(x, y) for x in range(3) for y in range(3)]

# 字典/集合推导式
d = {k: v for k, v in [("a", 1), ("b", 2)]}
s = {x for x in [1, 1, 2, 2, 3]}           # {1, 2, 3}
```

---

## 7. 字符串

### 7.1 定义方式与引号区别

```python
# 定义字符串的三种方式
s1 = '单引号'         # 单行
s2 = "双引号"         # 单行
s3 = '''三单引号'''    # 可多行
s4 = """三双引号"""    # 可多行

# 单引号和双引号完全等价，无功能差异
s1 = 'hello'
s2 = "hello"
s1 == s2            # True
type(s1) == type(s2)  # True 都是 str

# 区别：字符串内含某种引号时，用另一种包裹可免转义
s1 = "I'm a student"     # 双引号包裹，内部直接用单引号
s2 = 'He said "hi"'      # 单引号包裹，内部直接用双引号

# 等价的转义写法（用 \ 转义）
s3 = 'I\'m a student'    # \' 转义单引号
s4 = "He said \"hi\""    # \" 转义双引号

# 引号必须成对，不能一单一双
# s = "hello'   # 报错

# 三引号：多行字符串，内部可含单双引号
s = """
I'm a "student"
可以换行
"""
s = '''也可以用三个单引号'''

# 字符串不能直接跨行（除三引号外）
# s = "abc
# def"            # 报错
s = "abc\ndef"          # 用 \n 换行
s = ("abc"
     "def")             # 用括号续行 -> 'abcdef'
```

### 7.2 转义字符与原始字符串

```python
# 常用转义字符
# \n  换行
# \t  制表符
# \\  反斜杠
# \'  单引号
# \"  双引号
# \r  回车
print("a\nb\tc")        # a 换行 b 制表 c

# r 字符串：原始字符串，取消转义（路径、正则常用）
path = r"C:\new\folder"  # 原样输出，\n 不当换行
print(path)              # C:\new\folder

# 连续的字符串字面量自动拼接
s = "Hello" "World"      # 'HelloWorld'
s = "Hello" ' ' "World"  # 'Hello World'
```

### 7.3 索引与切片

```python
s = "Hello, World"
# 索引对应关系（两种访问方式）
# 字符:     H   e   l   l   o   ,       W   o   r   l   d
# 正向:     0   1   2   3   4   5   6   7   8   9  10  11   （从 0 开始，从左往右）
# 负向:   -12 -11 -10  -9  -8  -7  -6  -5  -4  -3  -2  -1   （从 -1 开始，从右往左）

# 1. 正向索引：从 0 开始
s[0]        # 'H'  第1个字符
s[1]        # 'e'  第2个字符
s[7]        # 'W'

# 2. 负向索引：从 -1 开始
s[-1]       # 'd'  最后1个字符
s[-2]       # 'l'  倒数第2个
s[-6]       # 'W'

# 越界会报错 IndexError
# s[100]    # 报错
# s[-100]   # 报错

# 切片 [起:止:步]  左闭右开
s[0:5]      # 'Hello'
s[:5]       # 'Hello'
s[7:]       # 'World'
s[::2]      # 'HloWrd' 步长2
s[::-1]     # 'dlroW ,olleH' 反转

# nums[::-1] 原理：切片格式 [起:止:步]
# 起止省略 = 取全部元素，步长 -1 = 从右往左取
# 组合 = 全部元素倒序 = 反转
# 例：[1,2,3,4,5][::-1] -> [5,4,3,2,1]
# 字符串同理："abc"[::-1] -> "cba"
```

### 7.4 拼接

```python
"a" + "b"        # 'ab'
"a" * 3          # 'aaa' 重复
",".join(["a", "b", "c"])   # 'a,b,c'

# + 只能连接字符串，含其他类型会报错
# "age:" + 18                # TypeError
"age:" + str(18)            # 'age:18'  需先转 str

# 推荐用 f-string，无需手动转换
age = 18
f"age:{age}"                # 'age:18'
```

### 7.5 占位符三种方式

```python
# 1. % 占位符（旧式）
"%s" % "abc"        # 字符串
"%d" % 18           # 整数
"%f" % 3.14         # 浮点数
"%.2f" % 3.14159    # 3.14  保留2位小数
"%x" % 255          # ff   十六进制
"%o" % 8            # 10   八进制
"%c" % 65           # A    ASCII 转字符
"%%"                # %    输出 % 本身
"%5d" % 1           # '    1' 宽度5右对齐
"%-5d|" % 1         # '1    |' 左对齐
"%05d" % 1          # '00001' 补零

# 2. {} + format()
"{} {}".format("a", "b")        # 'a b'
"{0}{1}{0}".format("a", "b")    # 'aba' 按索引
"{name}".format(name="张三")      # 命名参数
"{:>10}|".format("a")           # 右对齐宽10
"{:<10}|".format("a")           # 左对齐
"{:^10}|".format("a")           # 居中
"{:0>5}".format(42)             # '00042' 补零
"{:.2f}".format(3.14159)        # '3.14'
"{:,}".format(12345)            # '12,345' 千分位
"{:b}".format(10)               # '1010' 二进制

# 3. f-string（推荐）{} 内可直接嵌变量和表达式
name = "马涛"
age = 18
hobby = "java"
print(f"dajiahao{name},{age},{hobby}")   # dajiahao马涛,18,java

f"{name}今年{age}岁"                     # '马涛今年18岁'
f"{age+1}"                               # '19'  表达式
f"{3.14159:.2f}"                         # '3.14'
f"{42:0>5}"                              # '00042'
f"{12345:,}"                             # '12,345'
```

### 7.6 常用方法

```python
s = "  Hello, World  "
s.strip()              # 'Hello, World' 去两端空白
s.lstrip()             # 去左空白
s.rstrip()             # 去右空白
s.lower()              # 全小写
s.upper()              # 全大写
s.title()              # 单词首字母大写
s.capitalize()         # 首字母大写
s.swapcase()           # 大小写互换
s.replace("o", "0")    # 替换
s.split(",")           # 分割成列表
s.split(",", 1)        # 只分割1次
s.find("World")        # 找到返回索引，找不到 -1
s.index("World")       # 找到返回索引，找不到报错
s.count("l")           # 出现次数
s.startswith("He")     # 是否以..开头
s.endswith("ld")       # 是否以..结尾
s.isdigit()            # 是否全数字
s.isalpha()            # 是否全字母
s.zfill(5)             # 补零到5位 '00123'

# 判断成员
"H" in s               # True

# 长度
len(s)

# 遍历
for c in "abc":
    print(c)

# 多行字符串保持格式
text = f"""
姓名：{name}
年龄：{age}
"""
```
