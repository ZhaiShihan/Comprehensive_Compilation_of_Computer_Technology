### 壹  数据类型与基本语法

#### 述：
##### 求鱼须当向水中，缘木难得世人明。
##### 受尽爬揭鱼未晓，岂道此心非云顶？

###### · 基本语法规则：
1. 注释以井号“#”开头
2. Python 的语法采用缩进的方式
	1. 除注释外每一行都是一个语句
	2. 当语句以冒号“:”结尾时，缩进的语句视为代码块

· <font color="#00ffb0">程序 1 - 1</font>：Python 基本语法
```Python
# 判断一个数字是不是非负数
a=88
if a>=0:
	print('Yes.')
else:
	print('No.')
# 程序将会输出：Yes.
```

###### · print() 函数：
· `print()` 函数由两部分组成：
1. 指令 print
2. 指令的执行对象，在 print 后面的括号里的内容
· `print()` 函数的执行流程：
1. 向解释器发出指令，打印后面的内容
2. 解释器把代码解释为计算机能读懂的机器语言
3. 计算机执行完后打印结果
· 注意：括号里面要输出的内容必须加**引号**（单引号和双引号都可以（三引号也可以））：
```Python
print('Yes.')    # ok
print(No.)    # not ok
print("Maybe.")    # ok
```
· 选择单引号或双引号时，只需注意避免与字符串内部的引号冲突
· 例如，如果字符串中包含单引号，可以用双引号来定义字符串，反之亦然：
```Python
print("It's a beautiful day!")    # ok
print('He said, "Hello!"')    # ok
```

· 拓展：三引号通常用于定义多行字符串，或者当字符串内部包含单引号和双引号时避免转义：
```Python
print('''This is a
multi-line
string.''')    # ok

print("""She said, "It's a beautiful day!" and smiled.""")    # ok
```

· 注：print() 函数在调用结束后自动产生换行

###### · Python 的基本数据类型：
1. **字符串 string**：
· 字符串的识别是用引号实现的

· <font color="#00ffb0">程序 1 - 2</font>：字符串的识别用引号实现
```Python
str1 = 'hello world'  
str2 = "hello world"  
str3 = """hello world"""  
str4 = '''hello world'''  
print(str1)  
print(str2)  
print(str3)  
print(str4)
# 输出四行“hello world”

str5 = '''He said, "Hello!"'''  
str6 = "He said, \"Hello!\""  
print(str5)  
print(str6)
# 避免引号混淆的方法：
# 1. 使用不同引号加以区分
# 2. 可以利用“\”进行转义

str7='''H  
e  
l  
l  
o  
'''  
print(str7)
# 三引号 ``` 可以用于直接分行，输出结果为：
# H
# e
# l
# l
# o
```

· 当字符串中有中文的时候，通常在 Python 程序前面加上这样一行，告诉编译器按照 utf-8 编码，防止中文编译出现乱码：
```Python
# -*- coding: utf-8 -*-
```

2. **整数 integer**：
· 整数包括正整数、负整数和 0，是没有小数点的数字
· Python 中数字的定义不需要像 C++ 一样事先指明类型：
```Python
int_1 = 7  
print(int_1)    # 输出：7
# 这里 int_1 的数据类型是 int 型
```
· 注意：当数字被整数括起来的时候，它将变成一个字符串，而不再是一个数字

· 可以直接用一个计算式定义数字，如程序 1 - 3：

· <font color="#00ffb0">程序 1 - 3</font>：简单加减乘法定义一个整数
```Python
int_1 = 2 + 1  
int_2 = 2 - 1  
int_3 = 2 * 1
print(int_1)  
print(int_2)  
print(int_3)
# 输出：
# 3
# 1
# 2

int_4 = 2 / 1
int_5 = 1 / 2
print(int_4)
print(int_5)
# 输出：
# 2.0
# 0.5
# Python 自动将除法的结果识别成带小数点的数
```

· <font color="#00ffb0">程序 1 - 4</font>：利用 type() 函数查看数据类型
```Python
int_1 = 2 + 1  
int_2 = 2 - 1  
int_3 = 2 * 1  
int_4 = 2 / 1  
int_5 = 1 / 2  
print(type(int_1))  
print(type(int_2))  
print(type(int_3))  
print(type(int_4))  
print(type(int_5))
# 输出结果：
# <class 'int'>
# <class 'int'>
# <class 'int'>
# <class 'float'>
# <class 'float'>
```

3. **浮点数 float**：
· 在 Python 中用 float 表示浮点数的内置类型，没有“double”，Python 的 float 类型在内部通常使用双精度浮点数（64 为，符合 IEEE 754 标准）来表示，因此它的精度相当于其他编程语言中的“double”
· 计算机对浮点数的表达本身是不精确的：保存在计算机中的是二进制数，二进制对有些数字不能准确表达，只能非常接近这个数，因此在对浮点数做运算和比较大小的时候要格外小心：
```Python
print(0.55+0.41)
print(0.55+0.4)
print(0.55+0.411)
# 输出：
# 0.96
# 0.9500000000000001
# 0.9610000000000001
# 在浮点数运算的时候出现了微小的偏差
```

· Python 算术运算符：

| 运算符  | 表示                 | 例子                           |
| :--- | :----------------- | :--------------------------- |
| $+$  | 加                  | $2+1=3$                      |
| $-$  | 减                  | $1-2=-1$                     |
| $*$  | 乘                  | $1*2=2$                      |
| $/$  | 除                  | $1/2=0.5$                    |
| $\%$ | 取模——返回除法的余数        | $5\% 2=1$                    |
| $**$ | 幂——返回 $x$ 的 $y$ 次幂 | $2**3=8$                     |
| $//$ | 取整除——返回商的整数部分      | $11//2=5$<br>$11.0//2.0=5.0$ |

4. **布尔值 bool**：
· 布尔值只有两种取值：`True` 和 `False`（注意*首字母大写*）
· 布尔值可以用 `and`，`or` 和 `not` 来运算：
1. `and` 表示与运算，只有所有条件满足时才满足
2. `or` 表示或运算，只要有一个条件满足则满足
3. `not` 表示非运算，单目运算，用于 `True` 和 `False` 的互换

5. **空值 None**：
· 在 Python 中，空值作为一种特殊值，用 `None` 表示
```Python
p=None  
print(type(p))
# 输出：<class 'NoneType'>
```

###### · 基本数据类型转换：

| 方法         | 说明                              |
| :--------- | :------------------------------ |
| int(x)     | 将 x 转化成一个整数                     |
| float(x)   | 将 x 转化成一个浮点数                    |
| complex(x) | 将 x 转化成一个复数                     |
| str(x)     | 将 x 转化成一个字符串                    |
| repr(x)    | 将 x 转化成表达式字符串                   |
| eval(str)  | 用来计算在字符串中的有效 Python 表达式，并返回一个对象 |
| tuple(s)   | 将序列 s 转换为一个元组                   |
| list(s)    | 将序列 s 转换成一个列表                   |
| chr(x)     | 将整数转化为一个字符                      |
| unichr(x)  | 将整数转化为 Unicode 字符               |
| hex(x)     | 将整数转化为十六进制字符串                   |
| oct(x)     | 将整数转化为八进制字符串                    |

· 整数形式的字符串可以转化为整数：
```Python
str1 = '300'  
str2 = '100'  
print(str1 + str2)  
print(int(str1) + int(str2))
# 输出：
# 300100
# 400
```
· 注意：小数的字符串不能用 int() 转化为整数：
```Python
print(int('88.88'))    # 会报错
print(int(88.88))    # 输出向下取整：88
```

· <font color="#00ffb0">程序 1 - 5</font>：eval() 函数的表达式识别功能
```Python
# 简单的算术表达式  
result = eval('2 + 3 * 5')  
print(result)    # 输出：17

# 使用变量  
x = 10  
result = eval('x * 2')  
print(result)    # 输出：20

# 动态计算列表  
expression = '[i * 2 for i in range(5)]'  
result = eval(expression)  
print(result)    # 输出：[0, 2, 4, 6, 8]
```

###### · 变量的创建与赋值：
· 变量是用一个变量名表示，可以是任意数据类型
· 变量名必须是大小写英文、数字和下划线（_ ）的组合，且不能用数字开头

· Python 定义变量时不需要声明数据类型，可以把任意的数据类型赋值给变量
· 同一个变量可以反复赋值，可以是不同的数据类型
```Python
a = 'Hello.'
print(a)
a = 88
print(a)
# 输出：
# Hello.
# 88
```

· <font color="#00ffb0">程序 1 - 6</font>：变量的指向问题
```Python
a = 'Hello.'
b = a
a = 123
print(b)
# 输出：Hello.
```

· Python 允许同时为多个变量赋值：
```Python
a = b = c = 1    # ok
```
· 也可以为多个对象指定多个变量：
```Python
a, b, c = 1, 2, "Hello."
# 效果：a=1，b=2，c="Hello."
```

###### · 列表 List：
· 列表（List）是 Python 内置的一种数据类型，它是一种有序的集合，可以随时添加和删除其中的元素

· *列表的创建*：
```Python
name = ['张三', '李四', '王二麻子', '小淘气']
```
· 中括号里面的每一个数据都叫做元素，每个元素之间使用逗号分隔

· 列表中元素的数据类型不要求相同：
```Python
name_2 = ['张三', 123, "Li Si"]
print(name_2)
# 输出：['张三', 123, 'Li Si']
# 输出会采用默认格式，各元素之间以一个逗号和空格连接，字符串采用单引号，列表内容用中括号括起
```

· 列表的访问和 C++ 中数组相似，从 0 开始：
```Python
name_2 = ['张三', 123, "Li Si"]
print(name_2[1])
# 输出：123
```
· 还可以采用区间截取访问：
```Python
name_2 = ['张三', 123, "Li Si"]
print(name_2[0:2])    # 输出编号在区间 [0,2) 的数据
# 输出：['张三', 123]
```
· 注意：列表的区间访问是**左闭右开**区间，截取后的区间输出也是列表形式

· *列表的更新*：
1. 修改内容可以直接重新赋值
```Python
name_2 = ['张三', 123, "Li Si"]
name_2[1] = '李四'
print(name_2)    # 输出：['张三', '李四', 'Li Si']
```
2. 增添内容可以使用 append() 方法：（插入至 List 末尾）
```Python
name_2 = ['张三', 123, "Li Si"]
name_2.append('李四')
print(name_2)    # 输出：['张三', 123, 'Li Si', '李四']
```

· 使用 del 语句来删除列表的的元素：
```Python
list_1 = [1, 2, 3, 4, 5]  
del list_1[2]  
print(list_1)  
# 输出：[1, 2, 4, 5]
```

· **列表的运算**：

| Python                         | 结果                             | 描述         |
| :----------------------------- | :----------------------------- | :--------- |
| `len([1, 2, 3])`               | `3`                            | 计算元素个数     |
| `[1, 2, 3] + [4, 5, 6]`        | `[1, 2, 3, 4, 5, 6]`           | 组合         |
| `['Hi!'] * 4`                  | `['Hi!', 'Hi!', 'Hi!', 'Hi!']` | 复制         |
| `3 in [1, 2, 3]`               | `True`                         | 判断元素是否在列表中 |
| `for x in [1, 2, 3]: print(x)` | `1\ 2\ 3\ `                    | 迭代         |

· **列表相关函数**：

| 方法                        | 描述                        |
| :------------------------ | :------------------------ |
| `len(list)`               | 列表元素个数                    |
| `max(list)`               | 返回列表元素最大值                 |
| `min(list)`               | 返回列表元素最小值                 |
| `list(seq)`               | 将元素转换成列表                  |
| `list.append(obj)`        | 在列表末尾添加新的对象               |
| `list.count(obj)`         | 统计某个元素在列表中出现的个数           |
| `list.expand(seq)`        | 用新序列在末尾扩展原来的列表            |
| `list.index(obj)`         | 从列表中找出某个值第一个匹配项的索引位置      |
| `list.insert(index, obj)` | 将对象定点插入列表                 |
| `list.pop(index)`         | 无参默认删除列表尾端最后一个元素，并返回其值    |
| `list.remove(obj)`        | 删除列表中的一个元素（元素必须在列表中），不返回值 |
| `list.reverse()`          | 倒置列表顺序                    |
| `list.sort([func])`       | 对原列表进行排序（无参时默认从小到大排序）     |

· <font color="#00ffb0">程序 1 - 7</font>：模拟账户管理系统的 list 操作示例
```Python
# -*- coding: utf-8 -*-  
  
# 1. 一个产品，需要列出产品的用户，这时候就可以使用一个 list 来表示  
user = ['张三', '李四', '王二麻子']  
print('1. 产品用户：')  
print(user)  
  
# 2. 如果需要统计有多少个用户，len() 函数可以可以 list 里元素的个数  
len(user)  
print('\n2. 统计有多少个用户：')  
print(len(user))  
  
# 3. 用索引来访问 list 中每一个位置的元素，索引从 0 开始  
print('\n3. 查看具体的用户：')  
print(user[0] + ',' + user[1] + ',' + user[2])  
  
# 4. 在原有的 list 末尾加一个用户  
user.append('小淘气')  
print('\n4. 在末尾添加新用户：')  
print(user)  
  
# 5. 在第一位插入 VIP 用户  
user.insert(0, 'VIP user')  
print('\n5. 指定位置添加用户：')  
print(user)  
  
# 6. 删除末尾用户“小淘气”  
user.pop()  
print('\n6. 删除末尾用户：')  
print(user)  
  
# 7. 删除“张三”  
user.pop(1)  
print('\n7. 删除指定位置的 list 元素：')  
print(user)  
  
# 8. “王二麻子”要改名成“王五”  
user[2] = '王五'  
print('\n8. 把某个元素替换成别的元素：')  
print(user)  
  
# 9. 把用户账号也放进去  
new_user = [['VIP user', 111111], ['李四', 222222], ['王五', 333333]]  
print('\n9. 不同元素类型的 list 数据：')  
print(new_user)  
  
# 输出：  
# 1. 产品用户：  
# ['张三', '李四', '王二麻子']  
#  
# 2. 统计有多少个用户：  
# 3  
#  
# 3. 查看具体的用户：  
# 张三,李四,王二麻子  
#  
# 4. 在末尾添加新用户：  
# ['张三', '李四', '王二麻子', '小淘气']  
#  
# 5. 指定位置添加用户：  
# ['VIP user', '张三', '李四', '王二麻子', '小淘气']  
#  
# 6. 删除末尾用户：  
# ['VIP user', '张三', '李四', '王二麻子']  
#  
# 7. 删除指定位置的 list 元素：  
# ['VIP user', '李四', '王二麻子']  
#  
# 8. 把某个元素替换成别的元素：  
# ['VIP user', '李四', '王五']  
#  
# 9. 不同元素类型的 list 数据：  
# [['VIP user', 111111], ['李四', 222222], ['王五', 333333]]
```

###### · 元组 Tuple：
· 元组是不可变的有序序列，比 list 更安全

· *元组的创建*：
1. 用小括号表示元组，中间的元素用逗号隔开
2. 小括号可以省略
3. 当元组中只有一个元素的时候，这个元素的后面要有逗号
```Python
tuple_1 = (123, 'Hello', 456)
tuple_2 = 123, 'Hello', 456
tuple_3 = (123,)    # 小括号可以表示运算符，故元组要和运算式区分开：(123) 表示的是优先运算 123
tuple_4 = 123,    # 这种写法也可以识别成元组，但不常用
tuple_5 = ()    # 空元组
```

· *元组的访问*：
· 元组的访问方式和 list 相同：
```Python
tuple_1 = (123, 456, 'Hello world!')
print(tuple_1[1])
print(tuple_1)
# 输出：
# 456
# (123, 456, 'Hello world!')
```

· *元组的不变性*：
· 元组的不变性指的是每一个元素的指向永远不变，如下例：
```Python
list_1 = [1, 2, 3]
tuple_1 = (123, 'Hello', list_1)
```
· `tuple_1[0]` 和 `tuple_1[1]` 的指向恒为 123 与 'Hello'，`tuple_1[2]` 的指向恒为列表 list_1
· 列表 list_1 内部的元素可以变化：
```Python
list_1 = [1, 2, 3]
tuple_1 = (123, 'Hello', list_1)
list_1[0] = 5
list_1[1] = 4
print(tuple_1)
# 输出：(123, 'Hello', [5, 4, 3])
```

· *删除元组*：
· 元组中的元素是不允许删除的，但是可以删除整个元组
```Python
list_1 = [1, 2, 3]
tuple_1 = (123, 'Hello', list_1)
del tuple_1
```

· **元组的运算**：

| Python 表达式                     | 结果                             | 描述     |
| :----------------------------- | :----------------------------- | :----- |
| `len((1, 2, 3))`               | `3`                            | 计算元素个数 |
| `(1, 2, 3) + (4, 5, 6)`        | `(1, 2, 3, 4, 5, 6)`           | 连接     |
| `('Hi!',) * 4`                 | `('Hi!', 'Hi!', 'Hi!', 'Hi!')` | 复制     |
| `3 in (1, 2, 3)`               | `True`                         | 元素是否存在 |
| `for x in (1, 2, 3): print(x)` | `1\ 2\ 3\ `                    | 迭代     |

· **元组内置函数**：

| 方法           | 描述            |
| :----------- | :------------ |
| `len(tuple)` | 计算元组元素个数      |
| `max(tuple)` | 返回元组中元素的最大值   |
| `min(tuple)` | 返回元组中元素的最小值   |
| `tuple(seq)` | 将列表 seq 转换为元组 |

· <font color="#00ffb0">程序 1 - 8</font>：元组的各种简单操作示例
```Python
name_1 = ('一', '二', '三', '四', '五')  
name_2 = ('1', '2', '3', '4', '5')  
list_1 = [1, 2, 3, 4, 5]  
  
# 计算元素个数  
print(len(name_1))  
# 连接,两个元组相加  
print(name_1 + name_2)  
# 复制元组  
print(name_1 * 2)  
# 元素是否存在 (name1 这个元组中是否含有一点水这个元素)  
print('一' in name_1)  
# 元素的最大值  
print(max(name_2))  
# 元素的最小值  
print(min(name_2))  
# 将列表转换为元组  
print(tuple(list_1))  
  
# 输出：  
# 5  
# ('一', '二', '三', '四', '五', '1', '2', '3', '4', '5')  
# ('一', '二', '三', '四', '五', '一', '二', '三', '四', '五')  
# True  
# 5  
# 1  
# (1, 2, 3, 4, 5)
```

###### · 字典 Dictionary：
· 字典 dict 就相当于 C++ 中的 map，使用键-值（key-value）存储数据，具有极快的查找速度
· 字典是另一种可变容器模型，且可存储任意类型对象

· *字典的创建*：
· 字典的每个键值对用<font color="#ffc000">冒号</font>分割，每个对之间用<font color="#ffc000">逗号</font>分割，整个字典包括在<font color="#ffc000">花括号</font>中，格式如下所示：
```Python
dict_1 = {key_1: value_1, key_2: value_2}
```
· 注意：键必须是唯一的，但值则不必；值可以取任何数据类型，但键必须是不可变的

· *字典的访问*：
```Python
name_id = {'张三': 121212, '李四': 343434, '王二麻子': 565656}
print(name_id['李四'])
# 输出：343434
```
· 注意：如果字典中没有这个键，这样的查找会报错
· 注意：字典不是有序列表，不可以通过索引定位访问，只能通过键访问

· *字典的修改*：
```Python
name_id = {'张三': 121212, '李四': 343434, '王二麻子': 565656}
name_id['小淘气'] = 787878    # 增添作用
name_id['张三'] = 909090    # 修改作用
print(name_id)
# 输出：{'张三': 909090, '李四': 343434, '王二麻子': 565656, '小淘气': 787878}
```

· *字典的删除*：
· 字典中一个键值对就是一个元素，对于元素的删除就是整个键值对的删除
· 字典的删除可以使用 del 语句：
1. 可以直接 del 字典中的一个键值对
2. 可以将整个字典 del 掉
· 用 clear() 语句可以清空字典（但是字典本身不会被删除）

· <font color="#00ffb0">程序 1 - 9</font>：字典的 delete 和 clear
```Python
name_id = {'张三': 121212, '李四': 343434, '王二麻子': 565656}  
del name_id['张三']    # 删除个别元素  
print(name_id)  
name_id.clear()    # 清空所有元素  
print(name_id)  
del name_id    # 删除字典  
# 此时再编译这行会报错：print(name_id)  
  
# 输出：  
# {'李四': 343434, '王二麻子': 565656}  
# {}
```

· *使用字典的特殊注意事项*：
1. 字典的同一个键是不允许重复创建的，如果字典在创建时一个键被赋值两次，以最后一次为准：

· <font color="#00ffb0">程序 1 - 10</font>：字典在创建时将同一个键赋值两次
```Python
name_id = {'张三': 121212, '李四': 343434, '王二麻子': 565656, '张三': 787878}  
print(name_id)  
print(name_id['张三'])  
  
# 输出：  
# {'张三': 787878, '李四': 343434, '王二麻子': 565656}  
# 787878
```

2. 字典的键可以用用数字，字符串或元组充当，但是**不能使用列表**：
· 原理如 tuple 中的 list 可以改变内部值一样，字典是为了防止指向相同时 list 内部值变化导致键变化

3. dict 内部存放的顺序和 key 放入的顺序是没有任何关系

4. 与 list 相比较，dict 有以下几个特点：
	1. 查找和插入的速度极快，不会随着key的增加而变慢
	2. 需要占用大量的内存，内存浪费多
	3. 而 list 相反：
		1. 查找和插入的时间随着元素的增加而增加
		2. 占用空间小，浪费内存很少

· *字典的函数和方法*：

| 方法和函数            | 描述                       |
| :--------------- | :----------------------- |
| `len(dict)`      | 计算字典元素个数                 |
| `str(dict)`      | 输出字典可打印的字符串表示            |
| `type(variable)` | 返回输入的变量类型，如果变量是字典就返回字典类型 |
| `dict.clear()`   | 清空字典所有元素                 |
| `dict.copy()`    | 返回一个字典的浅拷贝               |
| `dict.values()`  | 以列表返回字典中的所有值（没有键）        |
| `popitem()`      | 随机返回并删除字典中的一对键和值         |
| `dict.items()`   | 以列表返回可遍历的（键，值）元组数组       |

· `dict.items()` 使用示例：
```Python
name_id = {'张三': 121212, '李四': 343434, '王二麻子': 565656}  
print(name_id.items())
# 输出：dict_items([('张三', 121212), ('李四', 343434), ('王二麻子', 565656)])
```

###### · 集合 Set：
· Set 是一个无序不重复元素集, 基本功能包括关系测试和消除重复元素
· set 和 dict 类似，但是 set 不存储 value 值

· *Set 的创建*：
· 创建一个 set 需要用一个 list 作为输入集合
```Python
set_1 = set([1, 2, 3])  
print(set_1)  
# 输出：{1, 2, 3}
```

· 集合的创建可以通过 set 函数的调用，也可以通过<font color="#ffc000">集合字面量</font>来简化：
```Python
set_1 = {1, 2, 3}
```
· 两种情况是只能使用 set 函数调用的：
1. 创建空 set：
```Python
set_1 = set()
```
2. 创建动态 set：
```Python
set_1 = set(range(1, 6))
```

· 集合可以用字符串创建：
```Python
set_1 = set('hello')
print(set_1)
# 输出：{'h', 'l', 'e', 'o'}
```

· *Set 的无重复性*：
```Python
set_1 = set([1, 2, 3, 2, 1])  
print(set_1)  
# 输出：{1, 2, 3}
```

· *Set 元素的添加*：
· 通过 add(key) 方法可以添加元素到 set 中，可以重复添加，但不会有效果

· <font color="#00ffb0">程序 1 - 11</font>：Set 的 add(key) 方法添加元素
```Python
set_1 = set([123, 456, 789])  
set_1.add(100)  
print(set_1)  
set_1.add(100)  
print(set_1)  
# 输出：  
# {456, 123, 100, 789}  
# {456, 123, 100, 789}
```

· *Set 元素的删除*：
· 通过 remove(key) 方法可以删除 set 中的元素

· <font color="#00ffb0">程序 1 - 12</font>：Set 的 remove(key) 方法删除元素
```Python
set_1 = set([123, 456, 789])  
set_1.remove(123)  
print(set_1)  
# 输出：  
# {456, 789}
```

· *Set 的运用*：
· 两个 set 可以做数学意义上的 union（并集）、intersection（交集）和 difference（差集）等操作：
1. 并集运算用“|”表示
2. 交集运算用“&”表示
3. 差集运算用“—”表示

· <font color="#00ffb0">程序 1 - 13</font>：Set 的并、差、交运算
```Python
set_1 = set('hello')  
set_2 = set(['p', 'y', 't', 'h', 'o', 'n'])  
print(set_1)  
print(set_2)  
  
# 交集（求两个 set 中相同的元素）  
set_3 = set_1 & set_2  
print(set_3)  
  
# 并集（合并两个 set 并去重）  
set_4 = set_1 | set_2  
print(set_4)  
  
# 差集（求在一个 set 而不在另一个 set 的元素）  
set_5 = set_1 - set_2  
set_6 = set_2 - set_1  
print(set_5)  
print(set_6)  
  
# 输出：  
# {'h', 'e', 'o', 'l'}  
# {'h', 'p', 't', 'o', 'y', 'n'}  
# {'h', 'o'}  
# {'h', 'p', 't', 'o', 'y', 'n', 'l', 'e'}  
# {'e', 'l'}  
# {'p', 'y', 'n', 't'}
```

· <font color="#00ffb0">程序 1 - 14</font>：用 set 去除海量列表里面的重复元素
```Python
list_1 = [111, 222, 333, 444, 111, 222, 333, 444, 555, 666]  
set_1 = set(list_1)  
print(set_1)  
# 输出：{555, 333, 111, 666, 444, 222}
```

z
### 贰  语句逻辑与函数

#### 述：
##### 兴起乘舟渡江河，负至深林层路磨。
##### 杜宇一声无前道，皆言此行求不得。
##### 白云悠悠慰我意，日出明光石上澈。
##### 莫惊前程是艰险，人生自古多逆波。

###### · 条件语句：
· 条件语句是通过一条或多条语句的执行结果（ True 或者 False ）来决定执行的代码块
· Python 程序语言指定任何非 0 和非“空（null）”值为 True，0 或者 null 为 False

· Python 语言有着严格的缩进要求，条件语句的基本格式为：
```Python
if 判断条件:
	执行语句…
else:
	执行语句…
```
· 注意：缩进是<font color="#ffc000">必不可少</font>的！

· 空字符串表示 False：
```Python
str_1 = ''  
if str_1:  
    print('hello')  
else:  
    print('no')
# 输出：no
```

· if 语句的多个判别条件使用 elif 实现，elif 即 “else if” 的简写：
```Python
if 判断条件1:
	执行语句1…
elif 判断条件2:
	执行语句2…
elif 判断条件3:
	执行语句3…
elif 判断条件4:
	执行语句4…
else:
	执行语句5…
```

· if 语句多个条件同时判断可以通过 <font color="#ffc000">and</font> 和 <font color="#ffc000">or</font> 实现：
```Python
a = 3  
b = 4  
if(a > 4 and b < 5) or (a <= 3 and b >= 4):  
    print('ok')
# 输出：ok
```

· if 嵌套语句：
```Python
a = 3
b = 4
if(a == 3):
	if(b == 4):
		print('ok')
	else:
		print('no')
else:
	print('no')
# 输出：ok
```

###### · 循环语句：
· 循环语句允许反复执行一个语句或语句组多次
· Python 提供了 for 循环和 while 循环

· *循环语句的控制*：

| 循环控制语句     | 描述                             |
| :--------- | :----------------------------- |
| `break`    | 在语句块执行过程中终止循环，并且跳出整个循环         |
| `continue` | 在语句块执行过程中终止当前循环，跳出该次循环，执行下一次循环 |
| `pass`     | 空语句，是为了保持程序结构的完整性              |

· *for 循环语句*：
· 基本语法格式：
```Python
for iterating_var in sequence:
   statements(s)
```

· <font color="#00ffb0">程序 1 - 15</font>：逐行打印已知字符串的每一个字符
```Python
str_1 = 'Hello'  
for x in str_1:  
    print(x)  
# 输出：  
# H  
# e  
# l  
# l  
# o
```

· <font color="#00ffb0">程序 1 - 16</font>：字典的循环打印只会输出 key 值
```Python
dict_1 = {1: 'a', 2: 'b', 3: 'c'}  
for x in dict_1:  
    print(x)  
# 输出：  
# 1  
# 2  
# 3
```

· *range() 函数*：
· for 循环常常与 range() 函数搭配使用
· `range(x)` 表示生成一个从 0 开始到 x-1 终止的序列
· `range(a, b)` 表示一个左闭右开的整数序列

· 例：`range(3)` 和 `range(0, 3)` 一样，表示序列：0，1，2
```Python
print(range(3))  
# 输出：range(0, 3)
```

· 使用 `range(n)` 多用来将一个代码段重复执行 n 次

· <font color="#00ffb0">程序 1 - 17</font>：判断区间【7，23）中有多少个 3 的倍数
```Python
count = 0  
for i in range(7, 23):  
    if i % 3 == 0:  
        count += 1  
    else:  
        continue  
print(count)
# 输出：5
```

· range() 函数还允许传入三个参数，第三个参数表示步长
· 如 `range(0, 10, 2)` 表示遍历 0，2，4，6，8（由于右开区间，故 10 不能取）

· <font color="#00ffb0">程序 1 - 18</font>：判断区间【18，360）之间有多少个数字既是 18 的倍数，又是 5 的倍数
```Python
count = 0  
for i in range(18, 360, 18):    # 直接以 18 为步长，减小循环次数，提高计算效率
    if i % 5 == 0:  
        count += 1  
    else:  
        continue  
print(count)
# 输出：3
```

· *while 循环语句*：
· while 的基本结构为：
```Python
while 循环条件:
	循环子句…
```

· <font color="#00ffb0">程序 1 - 19</font>：1 到 100 整数求和
```Python
sum = 0  
step = 0  
while step <= 100:  
    sum += step  
    step += 1  
print(sum)
# 输出：5050
```

· *for 循环和 while 循环的互化*：
· for 循环主要用在迭代可迭代对象的情况
· while 循环主要用在需要满足一定条件为真，反复执行的情况（死循环+break 退出等情况）
· 部分情况下，for 和 while 语句可以互化：

· <font color="#00ffb0">程序 1 - 20</font>：从 0 数到 9 的两种循环方式
```Python
# for 循环：  
for i in range(0, 10):  
    print(i)  
  
# while 循环：  
j = 0  
while j < 10:  
    print(j)  
    j += 1
```

· *嵌套循环*：
· for 循环的嵌套：
```Python
for iterating_var in sequence:
   for iterating_var in sequence:
      statements(s)
   statements(s)
```
· while 循环的嵌套：
```Python
while expression:
   while expression:
      statement(s)
   statement(s)
```

· 循环结构内还可以嵌套条件语句等其他结构

· <font color="#00ffb0">程序 1 - 21</font>：寻找【1，6）范围内的字典键值对
```Python
dict_1 = {1: 2, 2: 4, 4: 5,  
          3: 2, 5: 6, 7: 8, 6: 3}    # 目标字典  
for i in range(1, 6):  
    for j in range(1, 6):  
        if dict_1[i] == j:  
            print(i, j)  
# 输出：  
# 1 2  
# 2 4  
# 3 2  
# 4 5
```

· <font color="#00ffb0">程序 1 - 22</font>：自然数求和，当和大于 1000 时不再累加
```Python
sum = 0  
step = 1  
while sum < 1000:  
    sum += step  
    step += 1  
print(step - 1)  
print(sum)
# 输出：
# 45
# 1035
```

· <font color="#00ffb0">程序 1 - 23</font>：求 100 以内的奇数和
```Python
sum = 0  
step = 0  
while step <= 100:  
    if step % 2 == 1:  
        sum += step  
    step += 1  
print(sum)
# 输出：2500
```

· <font color="#00ffb0">程序 1 - 24</font>：二重循环法寻找 100 以内的所有质数
```Python
prime = []  
  
for i in range(2, 101):  
    prime.append(i)  
  
for num in range(2, 101):  
    for i in range(2, num):  
        if num % i == 0:  
            prime[num - 2] = 0  
  
for i in prime:  
    if i != 0:  
        print(i)
```

· *循环 + else 语句*：
· Python 中还提供了 for+else 和 while+else 的用法：
· else 中的语句会在循环正常执行完（即 for 不是通过 break 跳出而中断的）的情况下执行

· <font color="#00ffb0">程序 1 - 25</font>：for+else 语句寻找 100 以内的所有质数
```Python
prime = []  
for num in range(2, 101):  
    for i in range(2, num):  
        if num % i == 0:  
            break    # 当 break 触发后，else 语句将不会执行
    else:    # 没有触发过 break 意味着这是一个质数，执行 else 后面的语句
        prime.append(num)  
print(prime)
```

###### · 条件语句和循环语句的应用示例：

· <font color="#00ffb0">程序 1 - 26</font>：打印九九乘法表
```python
# -*- coding: UTF-8 -*-  
  
for i in range(1, 10):  
    for j in range(1, i+1):  
        print('{}x{}={}\t' .format(i, j, i*j), end='')
        # 格式化输出，用 .format 来解释内容；用 end 参数说明 print 结束后的输出（默认换行）
    print()
```

· 程序 1 - 26 的输出：
```
1x1=1	
2x1=2	2x2=4	
3x1=3	3x2=6	3x3=9	
4x1=4	4x2=8	4x3=12	4x4=16	
5x1=5	5x2=10	5x3=15	5x4=20	5x5=25	
6x1=6	6x2=12	6x3=18	6x4=24	6x5=30	6x6=36	
7x1=7	7x2=14	7x3=21	7x4=28	7x5=35	7x6=42	7x7=49	
8x1=8	8x2=16	8x3=24	8x4=32	8x5=40	8x6=48	8x7=56	8x8=64	
9x1=9	9x2=18	9x3=27	9x4=36	9x5=45	9x6=54	9x7=63	9x8=72	9x9=81
```

· <font color="#00ffb0">程序 1 - 27</font>：判断是否是闰年
```Python
year = int(input("请输入一个年份："))    # 输入 int 型的 year 值  
if (year % 4 == 0) and (year % 100 != 0) or year % 400 ==0:  
    print('{}年是闰年' .format(year))  
else:  
    print('{}年是平年' .format(year))
```

· 程序 1 - 27 的运行示例;
```
请输入一个年份：1933
1933年是平年
```

###### · 函数：
· 函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段
· 自定义函数的步骤：
1. 函数代码块以 `def` 关键词开头，后接函数标识符名称和圆括号`()`
2. 任何传入参数和自变量必须放在圆括号中间，圆括号可以用于定义参数
3. 函数的第一行语句可以选择性地使用文档字符串（用于存放函数说明）
4. 函数内容以冒号起始，并且缩进
5. return+表达式 结束函数，选择性地返回一个值给调用方（无表达式的 return 相当于返回 None）
· 函数语法示例：
```Python
def functionname( parameters ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

· <font color="#00ffb0">程序 1 - 28</font>：定义两数求和函数
```Python
def func_sum(s1, s2):  
    """两数之和"""    # 文档字符串
    return s1 + s2  
  
  
print(func_sum(5, 6))  
# 输出：11
```

· *参数类型检查*：
· 使用 `isinstance(x, type)` 语句判断函数传入参数类型是否符合规则

· <font color="#00ffb0">程序 1 - 29</font>：两数求和程序的传入参数类型检查
```Python
a = input()  
b = input()  
  
a = float(a)  
b = float(b)  
  
def func_sum(s1, s2):  
    if not(isinstance(s1, (int, float)) and isinstance(s2, (int, float))):  
        raise TypeError('参数类型错误')  
    return s1 + s2  
  
print(func_sum(a, b))
```

· 在程序 1 - 29 中可以发现，input 函数默认会将输入转成字符串，如果没有 float 的转换，任何输入下程序总会报错

· *多返回值*：
· Python 函数允许存在多个返回值

· <font color="#00ffb0">程序 1 - 30</font>：除法函数同时获得商与余数
```Python
def division(num1, num2):  
    # 求商与余数  
    a = num1 % num2  
    b = (num1 - a) / num2  
    return b, a  
  
num1, num2 = division(9, 4)  
tuple1 = division(9, 4)  
  
print(num1, num2)  
print(tuple1)

# 输出：
# 2.0 1
# (2.0, 1)
```

· <font color="#ffc000">Python 一次接受多个返回值的数据类型就是元组</font>

· *函数的参数*：
· 设置与传递参数是函数的重点，Python 函数对参数的支持非常灵活
· 主要的参数类型有：默认参数、关键字参数（位置参数）、不定长参数

1. 默认参数：
· Python 函数与 C++ 中函数都支持默认参数
· 注意：默认参数只能放在形参表尾，不能先声明默认：
```Python
def funa(a, b = 1)    # ok
def funb(a = 1, b)    # not ok!
```
· 默认参数在不指定传值的时候取设定默认值

· 注意：默认参数是不应该被修改的，当默认参数是可变的参数类型（如 list 和 dictionary）时要特殊注意，不当的使用会导致形参被误改
· <font color="#ffc000">在 Python 的函数传值中，字符串，整形，浮点型，tuple 是不可更改的对象，而 list，dict 等是可以更改的对象，类似于 C++ 中的引用传参</font>

· <font color="#00ffb0">程序 1 - 31</font>：list 作为默认参数时形参被误改
```Python
# -*- coding: UTF-8 -*-  
  
def print_info(a, b=[]):    # b是一个默认的空列表  
    print(b)  
    return b  
  
result = print_info(1)    # 第一次调用，b 使用默认的空列表  
# 输出：[]  
  
result.append('error')    # 修改了这个默认列表  
  
print_info(2)    # 第二次调用，b 仍然是被修改过的列表  
# 输出：['error']
```

· <font color="#00ffb0">程序 1 - 32</font>：使用 None 作为默认值避免 list 默认参数被误改
```Python
def print_info(a, b=None):
    if b is None:
        b = []
    return b

x = print_info(1)
print(x)
y = print_info(2)
print(y)
  
# 输出均为：[]
```

· <font color="#00ffb0">程序 1 - 33</font>：未赋值参数检查
```Python
_no_value = object()

def print_info_(a, b = _no_value):
    if b is _no_value:
        print('b 没有赋值')
    return

print_info_(1)
# 输出：b 没有赋值
```

· 程序 1 - 33 中的 object 代表 Python 中所有类的基类，可以创建实例，但是里面没有任何方法与数据
· 可以利用 object 这个特性测试同一性，来判断是否有值输入

2. 关键字参数（位置参数）：
· 一般情况下给函数传参要按顺序来，如果顺序不对应，就会传错值
· 在 Python 中可以通过参数名来给函数传递参数，不用关心参数列表定义时的顺序，称为“关键字参数”

· <font color="#00ffb0">程序 1 - 34</font>：关键字参数在传参顺序上的灵活性
```Python
def print_user_info(name, age, sex='男'):
    # 打印用户信息
    print('昵称：{}' .format(name), end=' ')
    print('年龄：{}' .format(age), end=' ')
    print('性别：{}' .format(sex))
    return

print_user_info(age=28, sex='女', name='张三妮')
# 输出：昵称：张三妮 年龄：28 性别：女
```

3. 不定长参数：
· 有些时候无法确定传入的参数个数，需要使用不定长参数
· Python 提供了一种元组的方式来接受没有直接定义的参数，这种方式在参数前边加星号`*`
· 如果在函数调用时没有指定参数，它就是一个空元组

· <font color="#00ffb0">程序 1 - 35</font>：不定长参数承接可变长度输入内容
```Python
def print_user_info(name, age, sex='男', *hobby):
    # 打印用户信息
    print('昵称：{}'.format(name), end=' ')
    print('年龄：{}'.format(age), end=' ')
    print('性别：{}'.format(sex), end=' ')
    print('爱好：{}'.format(hobby))
    return

print_user_info('张三妮', 28, '女', '喝茶', '打牌', '唠嗑')
# 输出：昵称：张三妮 年龄：28 性别：女 爱好：('喝茶', '打牌', '唠嗑')
```

4. 强制关键字参数：
· 关键字参数使用起来简单，不容易参数出错，有时我们希望只使用这一招传参方式
· 将强制关键字参数放到某个`*`参数或者单个`*`后面就能达到这种效果

· <font color="#00ffb0">程序 1 - 36</font>：强制关键字参数函数的定义与传参
```Python
# -*- coding: UTF-8 -*-  
  
def print_user_info(name, *, age, sex='男'):
    # 打印用户信息
    print('昵称：{}'.format(name), end=' ')
    print('年龄：{}'.format(age), end=' ')
    print('性别：{}'.format(sex))
    return

# 调用 print_user_info 函数
print_user_info(name='张三妮', age=28, sex='女')
print_user_info('王二麻子', age=33, sex='男')

# 这种写法会报错，因为 age 和 sex 这两个参数强制使用关键字参数：  
# print_user_info('张三妮', 28 ,'女')  
```

· *匿名函数*：
· Python 使用 <font color="#ffc000">lambda</font> 来创建匿名函数，也就是不再使用 def 语句这样标准的形式定义一个函数，使得短小函数的创建更加的便捷
· 匿名函数主要有以下特点：
1. lambda 只是一个表达式，函数体比 def 简单很多
2. lambda 的主体是一个表达式，而不是一个代码块，仅能在 lambda 表达式中封装有限的逻辑进去
3. lambda 函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数

· 示例：
```Python
sum1 = lambda num1, num2: num1 + num2  
print(sum1(1, 2))  
# 输出：3
```

· 尽管 lambda 表达式允许你定义简单函数，但是它的使用是有限制的：只能指定单个表达式，它的值就是最后的返回值；不能包含其他的语言特性，包括多个语句、条件表达式、迭代以及异常处理等等

· 注意：<font color="#ffc000">lambda 表达式中自由变量在运行时绑定值</font>，而不是定义时就绑定，这与函数的默认值参数定义是不同的：
```Python
# -*- coding: UTF-8 -*-  
  
num2 = 100  
sum1 = lambda num1: num1 + num2  
  
num2 = 10000  
sum2 = lambda num1: num1 + num2  
  
print(sum1(1))  
print(sum2(1))  
  
# 输出：  
# 10001  
# 10001
```


### 叁  迭代

#### 述：
##### 百步寻江看潮升，一层一层浪不穷。
##### 世历三古多经传，代代皆有圣贤成。

###### · 迭代与迭代器：
· Python 的 for 循环的抽象程度很高，不仅适用于 list 或 tuple，还可以作用于其他可迭代对象上
· 在 Python 中的可迭代对象不一定是有下标的，比如字符串

· <font color="#00ffb0">程序 1 - 37</font>：Python 的 for 循环迭代的高度抽象化
```Python
# 1. 迭代字符串  
for char in 'Hello world!':  
    print(char, end=' ')  
print('\n')  
  
# 2. 迭代 listlist1 = [1, 2, 3, 4, 5]  
for num1 in list1:  
    print(num1, end=' ')  
print('\n')  
  
# 3. 迭代 dictdict1 = {'name': '张三', 'age': 28, 'sex': '男'}  
for key in dict1:  
    print(key, end=' ')  
print('\n')  
for value in dict1.values():  
    print(value, end=' ')  
print('\n')  
  
# 4. 迭代 list 中的多变量元素  
list2 = [(1, 'a'), (2, 'b'), (3, 'c')]  
for x, y in list2:  
    print(x, y, end=' ')  
print('\n')  
  
# 输出：  
# H e l l o   w o r l d ! 
#
# 1 2 3 4 5 
#
# name age sex 
#
# 张三 28 男 
#
# 1 a 2 b 3 c
```

· 迭代是 Python 最强大的功能之一，是访问集合元素的一种方式
· 迭代器是一个可以记住遍历的位置的对象
· 迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束
· <font color="#ffc000">迭代器只能往前不会后退</font>

· 迭代器有两个基本的方法：`iter()` 和 `next()`，且字符串、列表或元组对象都可用于创建迭代器
· 迭代器对象可以使用常规 for 语句进行遍历，也可以使用 `next()` 函数来遍历

· <font color="#00ffb0">程序 1 - 38</font>：各种迭代器的创建与 for 循环遍历、next() 函数遍历
```Python
# 1. 字符串创建迭代器对象  
str1 = 'Hello world!'  
iter1 = iter(str1)  
  
# 2. list 创建迭代器  
list1 = [1, 2, 3, 4, 5, 6]  
iter2 = iter(list1)  
  
# 3. tuple 创建迭代器  
tuple1 = (1, 2, 3, 4, 5, 6)  
iter3 = iter(tuple1)  
  
# for 循环遍历迭代器对象：  
for x in iter1:  
    print(x, end=' ')  
print('\n--------------------')  
  
# next() 函数遍历迭代器  
while True:  
    try:  
        print(next(iter3), end=' ')  
    except StopIteration:  
        break  
  
# 输出：  
# H e l l o   w o r l d ! 
# --------------------  
# 1 2 3 4 5 6
```

###### · list 生成式：
· 要生成一个有 30 个元素的 list ，里面的元素为 1 - 30，可以这样写：
```Python
# -*- coding: UTF-8 -*-

list1 = list(range(1,31));
print(list1);
```

· <font color="#00ffb0">程序 1- 39</font>：使用一行输出代码打印九九乘法表
```Python
print('\n'.join([' '.join('%dx%d=%2d' % (x,y,x*y) for x in range(1,y+1)) for y in range(1,10)]))

# 输出：
# 1x1= 1
# 1x2= 2 2x2= 4
# 1x3= 3 2x3= 6 3x3= 9
# 1x4= 4 2x4= 8 3x4=12 4x4=16
# 1x5= 5 2x5=10 3x5=15 4x5=20 5x5=25
# 1x6= 6 2x6=12 3x6=18 4x6=24 5x6=30 6x6=36
# 1x7= 7 2x7=14 3x7=21 4x7=28 5x7=35 6x7=42 7x7=49
# 1x8= 8 2x8=16 3x8=24 4x8=32 5x8=40 6x8=48 7x8=56 8x8=64
# 1x9= 9 2x9=18 3x9=27 4x9=36 5x9=45 6x9=54 7x9=63 8x9=72 9x9=81
```

· list 生成式的两种创建方式：
```Python
[expr for iter_var in iterable] 
[expr for iter_var in iterable if cond_expr]
```
· 第一种方式是依靠迭代器的迭代
· 第二中方式是加入判断语句，筛选满足条件的元素放入 list 中

· <font color="#00ffb0">程序 1 - 40</font>：迭代器迭代生成平方数 list
```Python
# -*- coding: UTF-8 -*-
list1 = [x * x for x in range(1, 11)]
print(list1)

# 输出：[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

· <font color="#00ffb0">程序 1 - 41</font>：迭代器迭代生成偶数平方数 list
```Python
# -*- coding: UTF-8 -*-
list2 = [x * x for x in range(1, 21) if x % 2 == 0]
print(list2)

# 输出：[4, 16, 36, 64, 100, 144, 196, 256, 324, 400]
```

· <font color="#00ffb0">程序 1 - 42</font>：多重循环生成多维变量 list
```Python
# -*- coding: UTF-8 -*-
list3 = [(a, b, c) for a in range(1, 3) for b in range(1, 3) for c in range(1, 3)]  
print(list3)  
  
# 输出：[(1, 1, 1), (1, 1, 2), (1, 2, 1), (1, 2, 2), (2, 1, 1), (2, 1, 2), (2, 2, 1), (2, 2, 2)]
```

###### · 生成器：
· 受到内存限制，列表容量是有限的
· 例如：建一个包含 1000 万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了
· 在 Python 中，有一种一边循环一边计算的机制，称为<font color="#ffc000">生成器</font>：<font color="#ffc000">generator</font>
· 在 Python 中，使用了 `yield` 的函数被称为生成器（generator）
· 跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器
· 在调用生成器运行的过程中，每次遇到 `yield` 时函数会暂停并保存当前所有的运行信息，返回 `yield` 的值，并在下一次执行 `next()` 方法时从当前位置继续运行

· 创建生成器最简单的办法是把一个列表生成式的 `[]` 改成 `()`：
```Python
gen= (x * x for x in range(10))
print(gen)

# 输出：<generator object <genexpr> at 0x0000000002734A40>
```

· 生成器并不真正创建数字列表， 而是返回一个生成器，这个生成器在每次计算出一个条目后，把这个条目“产生” ( yield ) 出来
· 生成器表达式使用了<font color="#ffc000">“惰性计算”（延迟求值，lazy evaluation）</font>，即只有在检索时才被赋值（evaluated ），所以在列表比较长的情况下使用内存上更有效

· <font color="#00ffb0">程序 1 - 43</font>：生成器中元素的遍历
```Python
# -*- coding: UTF-8 -*-
gen = (x * x for x in range(10))

for num in gen:
    print(num)
  
# 输出：（0 到 81 的打印，一个数字占一行）
```

· 以函数形式实现生成器：
```Python
def fun():
	for i in range(1, 10):
		print i

fun()

改为：

def fun():
	for i in range(1, 10):
		yield i

print(fun())
# 输出：<generator object fun at 0x0000000002534A40>
```

· <font color="#00ffb0">程序 1 - 44</font>：计算斐波那契数列的生成器：
```Python
# -*- coding: UTF-8 -*-  
def fibon(n):  
    a = b = 1  
    for i in range(n):  
        yield a  
        a, b = b, a + b  
  
  
for x in fibon(10):  
    print(x, end=' ')  
  
# 输出：1 1 2 3 5 8 13 21 34 55
```

· 生成器和普通函数的执行流程不同：
	· 函数是顺序执行，遇到 return 语句或者最后一行函数语句就返回
	· 变成 generator 的函数，在每次调用 next() 的时候执行，遇到 yield 语句返回，<font color="#ffc000">再次执行时从上次返回的 yield 语句处继续执行</font>

· <font color="#00ffb0">程序 1 - 45</font>：生成器函数再次执行时从上次返回的 yield 处继续执行
```Python
# -*- coding: UTF-8 -*-  
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield 3
    print('step 3')
    yield 5


o = odd()
print(next(o))
print(next(o))
print(next(o))
# print(next(o))
# 生成器是只能执行一次的，这里如果再执行一个 print 语句则这一行会被报错
# 通常在 generator 函数中都要对错误进行捕获

# 输出：
# step 1
# 1
# step 2
# 3
# step 3
# 5
```

· <font color="#00ffb0">程序 1 - 46</font>：利用生成器打印杨辉三角
```Python
# -*- coding: UTF-8 -*-
def triangles():  # 杨辉三角形
    l1 = [1]
    while True:
        yield l1
        l1.append(0)
        l1 = [l1[i-1] + l1[i] for i in range(len(l1))]


n = 0
for t in triangles():
    print(t)
    n = n + 1
    if n == 10:
        break

# 输出：
# [1]
# [1, 1]
# [1, 2, 1]
# [1, 3, 3, 1]
# [1, 4, 6, 4, 1]
# [1, 5, 10, 10, 5, 1]
# [1, 6, 15, 20, 15, 6, 1]
# [1, 7, 21, 35, 35, 21, 7, 1]
# [1, 8, 28, 56, 70, 56, 28, 8, 1]
# [1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
```

###### · 反向迭代：
· <font color="#00ffb0">程序 1 - 47</font>：list 的正向迭代和反向迭代
```Python
list1 = [1, 2, 3, 4, 5]  
for num1 in list1:  
    print(num1, end=' ')  
for num2 in reversed(list1):  
    print(num2, end=' ')

# 输出：
# 1 2 3 4 5 5 4 3 2 1 
```

· 反向迭代仅仅当对象的大小可预先确定或者对象实现了 `__reversed__()` 的特殊方法时才能生效
· 如果两者都不符合，那你必须先将对象转换为一个列表才行

· <font color="#00ffb0">程序 1 - 48</font>：对象实现了 `__reversed__()` 的特殊方法时的双向迭代
```Python
# -*- coding: UTF-8 -*-  
  
class Countdown:  
    def __init__(self, start):  
        self.start = start  
  
    def __iter__(self):  
        # 前向迭代  
        n = self.start  
        while n > 0:  
            yield n  
            n -= 1  
  
    def __reversed__(self):  
        # 反向迭代  
        n = 1  
        while n <= self.start:  
            yield n  
            n += 1  
  
  
for rr in reversed(Countdown(30)):  # 正向迭代  
    print(rr, end=' ')  
for rr in Countdown(30):  # 反向迭代  
    print(rr, end=' ')
```

###### · 同时迭代多个序列：
· 为了同时迭代多个序列，使用 `zip()` 函数

· <font color="#00ffb0">程序 1 - 49</font>：使用 zip() 函数同时迭代多个序列
```Python
# -*- coding: UTF-8 -*-  
  
names = ['1a', '2b', '3c']  
ages = [40, 50, 60]  
for name, age in zip(names, ages):  
    print(name, age)  
  
# 输出：  
# 1a 40  
# 2b 50  
# 3c 60
```

· 遍历的多个序列如果长度不一致，以最短的为标准，遍历完后就结束

· 利用 `zip()` 函数，我们还可把一个 key 列表和一个 value 列表生成一个 dict（字典）

· <font color="#00ffb0">程序 1 - 50</font>：利用 zip() 函数将两个列表合成一个字典
```Python
# -*- coding: UTF-8 -*-  
  
names = ['1a', '2b', '3c']  
ages = [40, 50, 60]  
  
dict1 = dict(zip(names, ages))  
  
print(dict1)  
  
# 输出：{'1a': 40, '2b': 50, '3c': 60}
```

· 注：`zip()` 是可以接受多于两个的序列的参数，不仅仅是两个


### 肆  Python 逻辑与迭代综合问题

#### 述：
#####
#####

###### 1. 寻找符合条件的六位完全平方数：
· 有两个三位数 $A$ 和 $B$，满足 $B=A-10$，现由这两个三位数组合成一个六位数
· 数字 $A$ 的百位是新六位数的十万位，十位为新六位数的万位，个位为新六位数的千位
· 数字 $B$ 的百位、十位、个位分别对应新六位数的百位、十位、个位
· 现要求这个新六位数是完全平方数，则这样的六位数有多少个？

· <font color="#00ffb0">程序 1 - 51</font>：寻找符合 “$A\times 1000+A-10$” 条件的六位完全平方数（$A$ 是三位数）
```Python
# -*- coding: UTF-8 -*-  
def complete_sqrt(a):  
    """检查是否为完全平方数"""  
    if not(isinstance(a, int)):  
        raise TypeError("complete_sqrt 参数类型错误!")  
    for i in range(315, 1000):  
        y = i * i  
        if y == a:  
            return True  
    return False  
  
for A in range(110, 1000):  
    B = A - 10  
    C = A * 1000 + B  
    if complete_sqrt(C):  
        print(C, end=' ')  
  
# 输出：139129 235225 266256 394384 574564 811801
```

###### 2. 寻找符符合条件的三位完全平方数：
· 一个三位数 $X$，它是一个完全平方数
· $X$ 的三位数之积，它在所有的三位完全平方数中是独一无二的
· $X$ 的三位数之和，至少存在另一个三位完全平方数的三位数和与之相等
· 现在要求出所有可能的 $X$ 值

· <font color="#00ffb0">程序 1 - 52</font>：寻找所有各位积'独特"而各位和不"独特"的三位完全平方数
```Python
# -*- coding: UTF-8 -*-
list1 = []


def add_3(x):
    """三位数之和"""
    a = x % 10
    b = (int(x / 10)) % 10
    c = int(x / 100)
    return a + b + c


def multi_3(x):
    """三位数之积"""
    a = x % 10
    b = (int(x / 10)) % 10
    c = int(x / 100)
    return a * b * c


for i in range(10, 32):
    list1.append([i*i, add_3(i*i), multi_3(i * i)])  
  
dic_add = {}  # 存储每个三位和的出现次数  
dic_mul = {}  # 存储每个三位积的出现次数  
  
for Index in range(len(list1)):  # 寻找三位数积是独一无二，且和不是独一无二的  
    dic_add[list1[Index][1]] = 0  
    dic_mul[list1[Index][2]] = 0  
  
for Index in range(len(list1)):  
    dic_add[list1[Index][1]] += 1  
    dic_mul[list1[Index][2]] += 1  
  
print("所有的三位完全平方数：")  
  
for Index in range(len(list1)):  
    print(list1[Index][0], "和:", list1[Index][1], "积:", list1[Index][2])  
  
print()  
print("其中满足条件的：")  
  
for Index in range(len(list1)):  
    if dic_add[list1[Index][1]] > 1 and dic_mul[list1[Index][2]] == 1 and list1[Index][1] >= 10:  
        print(list1[Index][0], "和:", list1[Index][1], "积:", list1[Index][2])  
  
# 输出：  
# 所有的三位完全平方数：  
# 100 积: 1 和: 0  
# 121 积: 4 和: 2  
# 144 积: 9 和: 16  
# 169 积: 16 和: 54  
# 196 积: 16 和: 54  
# 225 积: 9 和: 20  
# 256 积: 13 和: 60  
# 289 积: 19 和: 144  
# 324 积: 9 和: 24  
# 361 积: 10 和: 18  
# 400 积: 4 和: 0  
# 441 积: 9 和: 16  
# 484 积: 16 和: 128  
# 529 积: 16 和: 90  
# 576 积: 18 和: 210  
# 625 积: 13 和: 60  
# 676 积: 19 和: 252  
# 729 积: 18 和: 126  
# 784 积: 19 和: 224  
# 841 积: 13 和: 32  
# 900 积: 9 和: 0  
# 961 积: 16 和: 54  
#  
# 其中满足条件的：  
# 289 和: 19 积: 144  
# 484 和: 16 积: 128  
# 529 和: 16 积: 90  
# 576 和: 18 积: 210  
# 676 和: 19 积: 252  
# 729 和: 18 积: 126  
# 784 和: 19 积: 224  
# 841 和: 13 积: 32
```

