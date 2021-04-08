# P4P/Python3.x
程序员学Python笔记

## chap1 
#### 1 算数运算
 
> 取余：整点数和浮点数都能取余（p2）
 
> 规则：余数(r) = 被除数(a) - n*(a // n)
 
```
-7 = 123 % （-10）
   = 123 - (-10)(123 // -10) 
   = 123 - (-10)(-13) # // 向下取整
 
 7 = -123 % 10
   = -123 - 10（-123 // (-10)）
   = -123 - 10*12
  
 -3 = -123 % (-10)
```
> 负号与乘幂
```
-5 ** -3 # -0.008
```

> Python取整——向上取整、向下取整、四舍五入取整、向0取整 <br>
> refer to: https://blog.csdn.net/weixin_41712499/article/details/85208928 <br>
向上取整: math.ceil()
```
from math import ceil
ceil(-0.5) # 0
ceil(-0.9) # 0`
ceil(0.3) # 1
```
向下取整: floor()
```
from math import floor
floor(-0.3) # -1
floor(0.9) # 0
```
银行家舍入取整: round(number)
```
# 四舍五入考虑：
# .5后非空进1
# .5后为空：5前为偶舍去，为奇进1
round(-2.5) # -2 
round(-1.5) # -2
```
向0取整: int()
```
# 丢掉小数，取整数部分
int(-0.9) # 0
int(0.5) # 0
```
除法取整: //(向下取整)
```
(-1) // 2  # -0.5 to -1
(-3) // 2  # -1.5 to -2
1 // 2    # 0.5 to 0
3 // 2    # 1.5 to 1
```

Python 浮点数 <br>
Python能够处理任意大的整数，精度有限的双精度浮点数（16~17位十进制精度）
> 小数通常以浮点数的形式存储 <br>
> 十进制形式 34.6 <br>
> 指数形式 2.1E5 = 2.1×105 <br>

#### 2 字符串操作
切片
- s[m:n]  # m >= n None
- s[m:n:d] # d是正数但 m >= n, d是负数但 n>=m 时 None

运算符优先级 <br>
(or < and < not)逻辑 < 比较运算符 < 算数

条件表达式，或称三元运算符 <br>
- 把 x 的绝对值赋值给 y ： y = `x if x >= 0 else -x`
- 条件表达式的优先级低于所有其他运算符
```
x = 2
y = 5

# z = (1 + x) if x > y else (y + 2)
z = 1 + x if x > y else y + 2 # 7

w = (1 + x) if x > y else (y + 2) # 8
```

程序控制和短路求值 <br>
a, b 是任意表达式，逻辑运算求值如下：
- a and b：先求a，如果a值为假，就以这个值作为整个 and 表达式的值；a不假再求b，以b值作为 and 表达式的值。
- a or b：先求a， 如果a值为真，就以这个值作为 or 表达式的值；否则求b，并以其值作为表达式的值。
- not a
```
3 and 0 # 0
0 or 'abc' # 'abc'
if x > 0 and y/x > 1: ...x...y
```

循环描述只在 for 语句开始时求值一次：
```
# i = 0, 1, 8
# n = 12
n = 3
for i in range(n):
    print(i**3)
    n = n + 3
```

#### 3 递归函数
递归函数：在函数体里调用自身，也是一种循环 <br>
> 思路： 基本情况直接给出结果，一般情况通过递归处理 <br>
例子 自然数指数乘幂: x**n
```
# 方法1 基本定义
def power(x, n):
    if n == 0:
        return 1
    else:
        return x * power(x, n-1)

# 方法2 条件表达式
def power(x, n):
    return 1 if n == 0 else x * power(x, n-1)

# 方法3 指数为奇转化为偶数，减少递归次数
def power(x, n):
    if n == 0:
        return 1
    elif n % 2 != 0: # n > 0 且为奇数
        return x * power(x, n-1)
    else:
        return power(x*x, n//2) # n > 0 且为偶数

# 循环实现（Python 既有循环也有递归结构）
def power(x, n):
    p = 1
    while n > 0:
        if n % 2 != 0:
            p *= x
            n -= 1
        else:
            x *= x
            n //= 2 # n 最终归结为奇数 1，转到 if 分支
    return p
```

递归、循环和执行时间 p27
> Fibonacci二路递归 循环不变式 平行赋值 <br>
> 递归定义，运算速度极慢
```

def fib(n):
    if n < 1: # F0 = 0
        return 0
    if n == 1: # F1 = 1
        return 1
    return fib(n - 1) + fib(n - 2) # Fn = F(n-1) + F(n-2) 
```
> improved: Loop + 平行赋值 <br>
 **循环不变式**证明 while LOOP p29 <br>
- 如果 每次判断循环条件时 f1 的值总是第 k 个斐波那契数，而 f2 的值总是第 k+1 个斐波那契数， 当 k=n 时循环结束，f1 就一定是第 n 个斐波那契数了。<br>
- 初始化中，f1==0，也即 F0，且 f2 的值是 F1，是下一个斐波那契数，所需关系成立。<br>
- 假设某次判断循环条件时，f1 的值是 Fk 且 f2 的值是Fk+1。循环体里的赋值使 f1 f2 分别变成 Fk+1 Fk+2，随后变量 k 的值加1，f1 f2 重新变成 Fk Fk+1。因此在下一次判断循条件时，所需关系仍然成立。<br>
```
# while LOOP
 def fib(n):
     if n < 0:
         return 0
     f1, f2 = 0, 1
     k = 0
     while k < n:
         f1, f2 = f2, f2 + f1
         k += 1
     return f1

# for LOOP
def fib(n):
    f1, f2 = 0, 1
    for k in range(n):
        f1, f2 = f2, f2 + f1
    return f1
    
# 递归
def fib0(f1, f2, k, n):
    if k > n:
        return f1
    else:
        return fib0(f2, f2+f1, k+1, n)
def fib(n):
    return fib0(0, 1, 1, n)
    
# 用表缓存 list F0~Fn
def gen_fibs(n):
    fibs = [0] * (n + 1)
    fibs[1] = 1
    for i in range(2, n+1):
        fibs[i] = fibs[i-1] + fibs[i-2]
    return fibs

def gen_fibs1(n):
    fibs = [0, 1]
    for i in range(2, n+1):
        fibs.append(fibs[-2] + fibs[-1])
    return fibs
 
fs = gen_fibs(20)

# 用字典缓存
def fib(n):
    fibs = {0:0, 1:1}
    
    def fib0(k):
        if k in fibs:
            return fibs[k]
        fibs[k] = fib0(k-2) + fib0(k-1)
        return fibs[k]
        
    return fib0(n)
```

#### 4 程序框架（函数嵌套p38 高阶函数p41、p69）
> 通用求根函数（立方根、平方根）
```
def appr_method(x, not_enough, improve):
    # 逼近计算框架函数，高阶函数
    if x == 0.0:
        return 0.0
    
    guess = x
    while not_enough(x, guess):
        guess = improve(x, guess)
    return guess
    
def not_enough(x, guess): # 通用判断逼近程度
    return abs((guess**3 - x) / x) > 1e-6
    
def cbrt(x):
    # 求立方根
    def improve(x, guess): # 函数嵌套
        retturn (2.0 * guess + x / guess / guess) / 3
        
    return appr_method(x, not_enough, improve)

def sqrt(x):
    # 求平方根
    def improve(x, guess):
        return (guess + x / guess) / 2
        
     return appr_method(x, not_enough, improve)
 ```
 
 > 匿名函数p45 lambda <br>
 > lambda 表达式不能出现赋值和函数定义 <br>
 > 可以有参数，参数即局部变量 <br>
 > 可以使用外围作用于中的变量 <br>
 ```
 def cbrt(x):
     return appr_method(x, not_enough, 
                        lambda x, y: (2.0 * y + x / y / y) / 3)
 ```
 
> 全局声明 <br>
> 希望在一个函数里修改全局定义的变量(global) 或 外围函数里的定义变量(nonlocal) <br>
> 函数内把 x 声明为全局变量， 如果执行到给 x 赋值时全局名字空间里没有 x ，解释器会把 x 加进全局名字空间并赋值 <br>
> e.g.cbrt: 统计在一次立方根计算中所有函数的调用次数 p52
```
count = 0
def cbrt(x):
    global count

    def not_enough(x, guess):
        global count
        count += 1
        return abs((guess**3 - x) / x) > 1e-6

    improve(x, guess):
        global count
        count += 1
        return (2.0 * guess + x / guess / guess) / 3

    count = 1

    if x == 0.0:
        return 0.0
    guess = x
    while not_enough(x, guess):
        guess = improve(x, guess)
        return guess
```

## chp2 数据的构造和组织
#### 1 list
#### 2 tuple
tuple: `t = 1, # t = (1, )` <br>
packing:`tp  = 123, 345, 'but', 567`, unpacking:`a, b, c, d = tp` p73 <br>
如果函数 fun(...) 返回一个二元组，`x, y = fun(...)`,则 x y 分别得到 fun 返回值里的两个成分。<br>
平行赋值 `x, y = 0, 1` 是左边拆分右边打包 <br>
> 有理数类p218 p78
```
class Rational:
    @staticmethod
    def _gcd(m, n):
        # 求最大公约数
        if n == 0:
            m, n = n, m
        while m != 0:
            m, n = n % m, m
        return n
    def __init__(self, num, den=1):
        if not (isinstance(num, int) and isinstance(den, int)):
            raise TypeError
        if den == 0:
            raise ZeroDivisionError
        sign = 1
        if num < 0:
            num, sign = -num, -sign
        if den < 0:
            den, sign = -den, -sign
        g = Rational._gcd(num, den) # 化简，无公约数
        self._num = sign * (num // g) # 规范 负号显示在分子上
        self._den = den // g
    def num(self):
        return self._num
    def den(self):
        return self._den
        
    # 特殊方法名 + * //
    def __add__(self, another):
        num = (self._num * another.den() +
               self._den * another.num())
        den = self._den * another.den()
        return Rational(num, den)
        
    def __mul__(self, another):
        return Rational(self._num * another.num(),
                        self._den * another.den())
                        
    def __floordiv__(self, another):
        if another.num() == 0:
            raise ZeroDivisionError
        return Rational(self._num * another.den(),
                        self._den * another.num())
     
    # other: -:__sub__, %:__mod__ ...
    
    # /:__truediv__ 有理数转换浮点数
    def __turediv__(self, another):
        if another.num() == 0:
            raise ZeroDivisionError
        rel = ((self._num * another.den())/
              (self._den * another.sun()))
        return rel
        
    # logical operation
    def __eq__(self, another):
        # num den 均通过 Rational 化简得到，不须通分比较
        return (self._num == another.num() and
                self._den == another.den())
        
    def __lt__(self, another):
        return (self._num * another.den() <
                self._den * another.num())
    # other: !=:__ne__, <=:__le__, >:__gt__, >=:__ge__
    
    def __str__(self):
        # print() 输出
        return str(self._num) + '/' + str(self._den)

if __name__ == "__main__":
    five = Rational(5)
    x = Rational(3, 5)
    print('Two third are', Rational(2, 3)) # Two third are 2/3
    y = five + x * Rational(5, 17) 
    if y > Rational(123, 11): print('It is large.')
    t = type(five) # <class '__main__.Rational'>
    if isinstance(five, Rational): print('It is OK.')
```
#### 3 序列操作
可变序列操作：
`s[0:0] = t` 在 s 头添加一段元素， t 是任意可迭代对象。
`s[len(s):len(s)] = t` 和 `s.extent(t)` 在 s 尾添加一段元素。
```
s = [1, 2, 3]
s[0:0] = '01'
print(s) # ['0', '1', 1, 2, 3]
```
#### 4 字符串和格式化p95
用 `print`输出时，如果实参不是字符串， `print`自动调用 `str`得到字符串。<br>
`s.format(*args, **keyargs)`
```
{1:->10s}
{price:10.2f}
{:<<10d}
```
```
from math import sin, cos

head = "{:<5}   {:<12s}  {:<12s}"
cotent = "{:5.3f}   {:12.10f}  {12.10f}"

def gen_table(start, end, step):
    print(head.format('x', 'sin(x)', 'cos(x)'))
    x = start
    while x < end:
        print(content.format(x, sin(x), cos(x)))
        x += step
        
gen_table(0.0, 1.05, 0.1)
```

#### 5 文件输入输出
`inf = open(...)`
文本文件可以看成行的序列，按行处理是最常见的方式。<br>
Python 把文本读文件对象看作可迭代对象，文本读文件对象可以直接转换为列表：
```
for line in inf:
    ...line...
inf.close()
```
> 文件操作函数p103：
```
f.tell()
f.seek(offset[, whence])
os.listdir(path='.')
os.chdir(path)
os.mkdir(path)
os.mkdirs(path)
os.rmdir(path)
os.remove(filename)
os.scandir(path='.')

import os
for entry in os.scandir():
    if entry.is_dir():
        print('DIctionary', entry.name)
    elif entry.is_file():
        print("Fileneme", entry.name)
    else:
        print("Something wrong")
```
> 统计Python代码总字符数、空格数p105
```
import os
def work_on_file(fname):
    t, b = 0, 0
    file = open(fname, encoding='utf8')
    for line in file:
        for c in line:
            t += 1
            if c.isspace():
                b += 1
    file.close()
    return t, b
def stat(path='.'):
    total, blank, other = 0, 0, 0
    os.chdir(path)
    for entry in os.scandir():
        if entry.is_file() and len(entry.name) > 3\
            and entry.name[-3:] == ".py":
            print("Work on file: ", entry.name)
            t, b = work_on_file(entry.name)
            total += t
            blank += b
            other += t - b
    return total, blank, other

print(stat())
```
> 词频统计p117
```
def word_stat(infname, stat_file):
    word_dict = {}
    num = 0
    textfile = open(infname)

    for line in textfile:
        word_list = line.split()
        for word in word_list:
            word = word.strip(",.':;!?()-_$/`~\"\\")
            if word == "":
                continue
            word_dict[word] = word_dict.get(word, 0) + 1
            num += 1
    textfile.close()

    outfile = open(stat_file, "w")
    for word in sorted(word_dict.keys()):
        outfile.write(word + ", " + str(word_dict[word]) + "\n")]
    outfile.close()
    return num, len(word_dict)
```
> 字频统计p111
```
def char_count(s, dic):
    for c in s:
        if not c.isalpha():
            continue
        c = c.lower()
        dic[c] = dic.get(c, 0) + 1
```


#### 5 Dict
具有迭代性质的对象：
> 迭代过程中不应增删处理字典元素，否则可能：继续迭代可能遗漏元素，也可能报 RuntineError
> 通过标准函数 iter 得到字典的迭代器后，处理中也不允许增删字典元素
```
dic.keys()
dic.values()
dic.items()

for k in dic.keys():
    ...k...dic[k]...
==
for k in dic: # 解释器自动调用 keys()
    ...k...dic[k]...
```
## chap 3 深入理解 Python
#### 1 copy 和共享
> 表拷贝创建一个新表，但只做最上面一层拷贝 <br>
> 元素共享，如被拷贝表中有可变元素，修改共享元素要注意相互影响 <br>
> 3X3矩阵p138
```
# [[0, 1, 2], [1, 2, 3], [2, 3, 4]]
a1 = [[i + j for i in range(3)] for j in range(3)]

a2 = a1.copy()

# a1 = [[5, 1, 2], [1, 2, 3], [2, 3, 4]]
# a2 = [[5, 1, 2], [1, 2, 3], [2, 3, 4]]
a1[0][0] = 5

# a1 = [[5, 1, 2], [7, 8, 9], [2, 3, 4]]
# a2 = [[5, 1, 2], [1, 2, 3], [2, 3, 4]]
a1[1] = [7, 8, 9]
```
> 变量元素共享
```
a1 = [1] * 10
a2 = [[1]] * 10
a1[1] = 2
a2[1][0] = 2

# a1 = [1, 2, 1, 1, 1, 1, 1, 1, 1, 1]
# a2 = [[2], [2], [2], [2], [2], [2], [2], [2], [2], [2]]
```
> 如果表元素是可变对象，对他做各种显式隐式拷贝时，须特别关注元素共享情况，如默认操作不合适时，应自己做元素拷贝 <br>
> Python默认只做一层拷贝，如果其中元素是可变对象，要当心了 <br>
> 例：生成一个表，其中包含10个空表
```
dlist = [[] for i range(10)] # 用描述式生成合适的表
dlist[0].append(1)
# dlist = [[1], [], [], [], [], [], [], [], [], []]
```
> 共享参数；通过参数修改调用的环境p142
```
def f(a, b):
    b = [8, 9]
    a[1] = 6
    return a
    
m = [1, 2, 3]
n = [4, 5]
s = f(m, n)

# 共享参数：在函数里通过参数修改共享的可变对象
# m = a = [1, 6, 3]

# n = [4, 5]
# b = [8, 9]
```
> 陷阱：*可变对象* 作为函数默认值 计算结果可能不符预期
```
def append5(a, b=[]):
    for i in range(5):
        b.append(a)
    return b
    
append5(1, []) # [1, 1, 1, 1, 1]
append5(1) # [1, 1, 1, 1, 1]
append5(2) # [1, 1, 1, 1, 1, 2, 2, 2, 2, 2]

# 解决办法
def append5(a, b=None):
    if b is None:
        b = []
    for i in range(5):
        b.append(a)
    return b
``` 
#### 2 逻辑判断条件误区与效率
```
# 死循环，条件永远为真
while list1 is mot []:
    x = list1.pop()
    ...x...
    
# 每次迭代创建一个空表
while list1 != []:
    x = list1.pop()
    ...x...

# 每次判断要一次 len() 开销
while len(list1) != 0:
    x = list1.pop()
    ...x...
    
# 效率高，消除额外开销
while list1:
    x = list1.pop()
    ...x...

# 整数 n
if not n: ...
if n == 0: ...
# 短路求值
if list1 and list1[0] > 1: ...
```
> 不变组合类型空对象永远只有一个 <br>
> 系统常量 Ture False None 具有唯一性
```
# 当 p 值为 None 时做一些事：
if p is None: ... # None 是不变对象，具有唯一性，此法最佳
if p == None: ... # 与上面语义等价
if not p: ...     # 歧义：p == 0 [] "" 时语句也执行
```
> *Python规定* p149 <br>
> 二元运算符总先求值其左边的运算对象 <br>
> 函数调用时先求出所需函数对象，而后从左到右逐个求值实参表达式 <br>
> 完成后做实参与形参的匹配，在执行函数体 <br>
```
# 先求值左边还是先求值右边？
x = [1, 2, 3]
print(x.pop() * 2 + x.pop())
```
#### 3 生成器函数，函数带有 `yeild x`
> 调用一次生成器函数返回一个迭代器对象 <br>
> range函数、生成器表达式 `(n**2 for n in range(10))`、map、filter 等 <br>
> 返回对象都与生成器类似，也可以对这种对象调用 `next()` <br> 
> 例子：*生成器实现*获取文件里的浮点数 
```
def read_floats(fname):
    infile = open(fname)
    for line in infile:
        for s in line.split():
            yield float(s)
    infile.close()
```
> *普通函数*同样功能 p165：
```
"""
模块 readfloats.py 
支持用户读入内容为浮点数的数据文件，采用缓冲方式
函数 open_floats 打开指定文件
每次调用函数 next_float 返回一个浮点数
读完文件中所有浮点数后，该函数返回 None
用一组全局变量记录文件对象和文件读入的状态
"""
infile = None
nlist = []
crt = 0

def open_floats(fname):
    global infile
    infile = open(fname)
def next_float(): # 缓冲式处理
    global nlist, crt
    if crt == len(nlist): # 当前行用完，再读一行
        line = infile.readline()
        if not line: # 空字符串，表示文件已经处理完
            infile.close()
            return None
        nlist = line.split()
        crt = 0
    x = nlist[crt] # 当前元素
    crt += 1
    return float(x)
```
#### 4 *闭包(closure)实现* 获取文件里的浮点数 p172
```
"""
read_floats 结束时，其值返回给 next_number
返回值包括 闭包二元组：
1~ 局部函数 next_float 定义生成的函数对象
2~ read_floats 本次调用建立的名字空间

每次调用函数 read_floats 将建立一个新的局部名字空间
返回的闭包二元组引用着它，其中封装着读入文件的状态
可以多次调用 read_floats 打开多个文件
不同文件读入互不干扰

闭包还用于定义装饰器
"""
def read_floats(fname):
    nlist = []
    infile = open(fname)
    crt = 0
    
    def next_float():
        nonlocal nlist, crt
        if crt == len(nlist):
            line = infile.readline()
            if not line:
                infile.close()
                return None
            nlist = line.split()
            crt = 0
        crt += 1
        return float(nlist[crt-1])
    return next_float # 函数返回局部定义的函数对象，即闭包
    
if __name__ == "__main__":
    next_number = read_floats("datafile.dat")
    for i in range(10):
        print(next_number())
    print("--" * 10)
```
> 闭包构造装饰器p314
```
def deco(fun):
    def wrapper(*args, **kwargs):
        print(fun.__name__+" stars.")
        x = fun(*args, **kwargs)
        print(fun.__name__+" end.")
        return x
    return wrapper

def decodeco(fun):
    def wrapper(*args, **kwargs):
        print(fun.__name__ + "......")
        x= fun(*args, **kwargs)
        print(fun.__name__ + "------")
        return x
    return wrapper

@decodeco
@deco
def func4(a, b, c, d, e):
    print(a + b*c - d + e)
    return(a + b*c -d + e)

func4(1, 2, 3, 4, 5)
# wrapper......
# func4 stars.
# 8
# func4 end.
# wrapper------
```
> 装饰器构造 日志文件(.log)p318
```
def log_func(fname):
    def deco(fun):
        def wrapper(*args, **kwargs):
            logfile.write(fun.__name__ + " stars.\n")
            logfile.flush()
            x = fun(*args, **kwargs)
            logfile.write(fun.__name__ + " ends.\n")
            logfile.flush()
            return x
        return wrapper
    if fname[-4:] != ".log":
        fname = fname + ".log"
    logfile = open(fname, 'w')
    return deco

log2 = log_func("logfile2")

@log2
def func3(a, b, c, d):
    return a + b * c - d
@log2
def func4(a, b, c, d, e):
    x = func3(a, b, c, d)
    y = func3(b, c, d, e)
    return x * y

x1 = func3(2, 3, 5, 1)
x2 = func3(2, 3, 4, 2)
a = func4(2, 3, 4, 2, 10)
b = func4(2, 3, 2, 5, 4)

print(x1 + x2, a - b)

# func3 stars.
# func3 ends.
# func3 stars.
# func3 ends.
# func4 stars.
# func3 stars.
# func3 ends.
# func3 stars.
# func3 ends.
# func4 ends.
# func4 stars.
# func3 stars.
# func3 ends.
# func3 stars.
# func3 ends.
# func4 ends.
```
#### 5 效率p204
时间复杂性、空间复杂性：以大 O 记法，表示一端计算空间或时间消耗的数量级 p193
```
O(1) 常量时间操作
O(n) 线性时间操作
O(log n) 对数复杂性
```
Python 程序运行中隐式代价分为：<br>
为支持程序语义，在程序运行中需要实际执行的一些内部操作<br>
与 *数据对象* 构造和使用的有关开销<br>
> 引用语义：变量、组合对象 `list 中不储存元素，而是存放元素的引用`，元素外置，导致每次使用变量多了一次内存访问<br>
> 自动化存储管理：引用计数技术回收垃圾对象<br>
> CPython 为避免重复创建最常用对象，事先准备一批对象，保证在程序中不再创建他们 `小整数和小字符串`<br>
> 减少大型对象创建，有可能提高程序性能p196<br>
> 尽可能避免无意义对象创建<br>
> `Python 标准文档 FAQ 一节对程序性能 performance 有专门讨论` <br>

标准组合类型的实现和操作效率p200<br>
    典型的性能错误：运行中不断创建临时性的组合数据对象，尤其是在循环递归等中，性能损失严重 <br>
    Python 标准组合对象采用连续的元素存储区 <br>
    可变对象的扩容是高代价操作，可能造成常规处理的短暂停息 <br>
组合类型结构： <br>
    一体式（不变类型）：`对象信息 + 元素存储区（元素内置or外置）` <br>
    分离式（可变类型）：`对象信息 + （） --> 元素存储区（元素内置or外置）` <br>
`list 对象信息 + （） --> 元素存储区（元素的标识符外置）` <br>
    扩容策略：等比扩容， append 分摊式时间复杂度O(1) <br>
    list1.clear() O(n),若触动元素回收机制，开销更大 <br>
    不会自动压缩储存，当大表删除元素小表合算时，只能另建新标 copy 旧表元素 <br>
`dict 对象信息 + （） --> 元素存储区（元素的标识符外置）` <br>
    hash表效率通常情况下检索是常量时间，特殊情况O(n) <br>
    一般而言 CPython 字典的良好性能是概率意义上的，没绝对保证 <br>
    若程序操作对时间要求特别严格，要考虑其他结构 <br>

## chap 4 面向对象编程
#### 1 类和对象
类定义，执行时建立他描述的 *类对象* 并约束于类名。类中函数定义设定其 *函数属性*，赋值语句设定其 *数据属性*。
实例化即创建类的实例对象。用一个初始化函数 `__init__` 定义 *实例* 的属性设置。
- 实例方法p217
  - r1 r2 是有理数，plus 是 Rational 里表示求和的实例方法，`r1.plus(r2)` 从对象 r1 出发以 r2 为参数调用 plus。
  - 定义实例方法时第一个形参通常用 self， 表示调用的函数对象(~应该是类对象，即有理数对象?）。
  - 执行 `r1.plus(r2)` 时，r1 约束到 self，其他实参约束到后面的形参。
- 静态方法 @staticmethod：局部定义非实例方法
  - 静态方法就是定义在类里的普通函数、局部函数，但其形参表里没有 self。
  - 可以从其类名出发调用，也可从该类的对象(r1._gcd?)出发来调用p218
- 类方法 @classmethod，实现与类有关的方法
  - 例子，类实例计数器p222
  - 参数表里至少有一个表示类的形参 cls
  - 类方法通过类名，以属性引用的形式调用
  - 执行时这个类约束到方法的 cls 参数，可以通过 cls 访问其他属性
```
# 客户管理器p227
class Customer:
    def __init__(self, name):
        self.name = name
        self._total = 0
    def pay(self, price):
        self._total += price
        return price
    def total(self):
        return self._total
        
class CustomerManager:
    """
    定义一个数据属性，用一个字典记录所有用户信息。
    几个类方法完成客户管理操作，
    包括加入新客户、客户购物累计金额、检查客户购物总金额。
    """
    _customers = {}
    
    @classmethod
    def new_customer(cls, name):
        if not isinstance(name, str):
            raise TypeError("Creat Record Error: ", name)
        cls._customers[name] = Customer(name)
        
    @classmethod
    def pay_price(cls, name, price):
        if (not isinstance(name, str) or not isinstance(price, float)):
            raise TypeError("Purshese Error: ", name, price)
        if name not in cls._customers:
            cls._customers[name] = Customer(name)
        return cls._customers[name].pay(price)
        
    @classmethod
    def check_total(cls, name):
        if name not in cls._customers:
            raise KeyError("Not this customer: ", name)
        return cls._customers[name].total()
        
# 继承例子：客户管理器扩充，加入 VIP 客户管理p239
# 如果派生类对象需要新属性，须覆盖基类 __init__ 函数，
# 在其中初始化所需新属性
# 还需要初始化基类的对象的那些属性（p232），比如：
# BaseClass.__init__/super().__init__()

class VIP(Customer):
    _discount = 0.98
   
    # 本初始化函数继承自基类 Customer，不能删除：
    # 第一，当 total < 1000.0，类 VIP 要用基类 Customer 初始化函数及其实例方法
    # 第二，当 total >= 1000.0，类 VIP 才正式用本类初始化函数及其方法，
    # 此时，total 参数值来源 pay_price(cls, name, price) 函数 total 变量，
    # 如果此时没有 total、 __init__(self, name, total)，check_total 返回值为 0，
    # 这里假设 VIP 都是有过交易的客户，他们当前的消费总额由参数 total 给定p239
    def __init__(self, name, total):
        super().__init__(name)
        self._total = total
    def pay(self, price):
        paid = round(price * VIP._discount, 2)
        self._total += price
        return paid

class VIPCusManager(CustomerManager):
    _VIP_price = 1000.0
    
    @classmethod
    def is_VIP(cls, name):
        if (name not in cls._customers or not isinstance(cls._customers[name], VIP)):
            return False
        return True
    
    @classmethod
    def pay_price(cls, name, price):
        if (not isinstance(name, str) or not isinstance(price, float)):
            raise TypeError("Purshese Error: ", name, price)
        if name not in cls._customers:
            cls._customers[name] = Customers[name]
        
        customer = cls._customers[name]
        paid = customer.pay(price)
        total = customer.total()
        
        if (not isinstance(customer, VIP) and total > cls._VIP_price):
            cls._customers[name] = VIP(name, total)
            
        return paid
```
- 同一个类里，方法属性调用：
  - 一个实例方法调用其他实例方法，须通过 `self.func(...)` 形式
  - 类数据/函数属性作用域不自动延伸到其内部嵌套的作用域，引用须基于 *类名采用属性引用* 的形式
- 类外调用
  - 类中按默认方式定义的函数可通过本类实例调用
  - @staticmethod @classmethod 除外
  - 类定义也是局部作用域，类外使用须采用基于类名的属性访问形式
- 屏蔽问题： `__init__` 里赋值的属性与类中方法同名，就会屏蔽同名实例方法
  - `Rational.__init__` 里若给 self.num 赋值，则 Rational.num 被屏蔽。

```
class Rational:
    """有理数类，
    通过最大公约数化简后得到最简形式有理数，
    并且规范有理数，±符号统一在分子上。
    定义有理数算数、逻辑运算"""
    
    @staticmethod
    def _gcd(m, n):
        # 求最大公约数
        if n == 0:
            m, n = n, m
        while m != 0:
            m, n = n % m, m
        return n
    def __init__(self, num, den=1):
        if not (isinstance(num, int) and isinstance(den, int)):
            raise TypeError
        if den == 0:
            raise ZeroDivisionError
        sign = 1
        if num < 0:
            num, sign = -num, -sign
        if den < 0:
            den, sign = -den, -sign
        g = Rational._gcd(num, den) # 化简，无公约数
        self._num = sign * (num // g) # 规范 负号显示在分子上
        self._den = den // g
    def num(self):
        return self._num
    def den(self):
        return self._den
        
    # 特殊方法名 + * //
    def __add__(self, another):
        num = (self._num * another.den() +
               self._den * another.num())
        den = self._den * another.den()
        return Rational(num, den)
        
    def __mul__(self, another):
        return Rational(self._num * another.num(),
                        self._den * another.den())
                        
    def __floordiv__(self, another):
        if another.num() == 0:
            raise ZeroDivisionError
        return Rational(self._num * another.den(),
                        self._den * another.num())
     
    # other: -:__sub__, %:__mod__ ...
    
    # /:__truediv__ 有理数转换浮点数
    def __turediv__(self, another):
        if another.num() == 0:
            raise ZeroDivisionError
        rel = ((self._num * another.den())/
              (self._den * another.sun()))
        return rel
        
    # logical operation
    def __eq__(self, another):
        # num den 均通过 Rational 化简得到，不须通分比较
        return (self._num == another.num() and
                self._den == another.den())
        
    def __lt__(self, another):
        return (self._num * another.den() <
                self._den * another.num())
    # other: !=:__ne__, <=:__le__, >:__gt__, >=:__ge__
    
    def __str__(self):
        # print() 输出
        return str(self._num) + '/' + str(self._den)
```
#### 2 继承
基类与派生类。派生类初始化中，如加入新属性须考虑基类继承的属性，通常是：
```
class DerivedClass(BaseClass):
    def __init__(self, x, y, ... ):
        BaseClass.__init__(...)
        x...y...      
```
动态约束原则p236
- 基于方法调用时， self 所指实例对象的类型确定被调函数（g()）
- Python 解释器在派生类中查找函数类别归属时，self 指向本派生类：
```
class B:
    def f(self):
        return self.g()
    def g(self):
        print("B.g is called.")

class C(B):
    def g(self):
        print("C.g is called.")
y = C()
y.f() # C.g is called.
```
Python 提供 super 方法实现多继承中如何指定基类功能：
- 默认形式：super().method()，派生类直接调用第一个基类的方法 BaseClass.method()
- super(C, o).method() 
- https://www.runoob.com/python/python-func-super.html
- 见 *客户管理器扩充* 例子

#### 3 review: *Class-LinkedList
```
#coding=utf-8

class SLNode:
    'single link node'
    __slots__ = ("elem", "next")

    def __init__(self, elem, next_=None):
        self.elem = elem
        self.next = next_

class LinkedListUnderflow(ValueError):
    pass

class LinkedList:

    Node = SLNode

    def __init__(self, inits=()):
        self._head = None
        if not inits:
            return
        try:
            it = iter(inits)
            p = type(self).Node(next(it))
            self._head = p
            for x in it:
                p.next = type(self).Node(x)
                p = p.next
            return
        except TypeError:
            print("Non-iterable is used in Initiating LinkedList.")
            raise
        except StopIteration:
            return

    def __bool__(self):
        return self._head is not None

    def __len__(self):
        p = self._head
        ln = 0
        while p:
            p = p.next
            ln += 1
        return ln

    def __str__(self):
        p = self._head
        s = []
        while p:
            try:
                s.append(str(p.elem))
            except TypeError:
                print("Elem in LinkedList cannot convert to str.")
                raise
            p = p.next
        return "[[{}]]".format(", ".join(s))

    def __iter__(self):
        p = self._head
        while p:
            yield p.elem
            p = p.next

    def __getitem__(self, key):
        if isinstance(key, int):
            if key < 0:
                raise IndexError("LinkedList does not support"
                                 " neg index", key)
            p = self._head
            i = 0
            while i < key:
                if p is None:
                    raise IndexError("Index in LinkedList is"
                                     " out of range", key)
                p = p.next
                i += 1
            return p.elem

        if isinstance(key, slice):
            res = LinkedList()
            p = self._head
            if not p: return res

            start = key.start or 0
            stop = key.stop
            step = key.step or 1
            # print(start, stop, step)
            
            if not (isinstance(start, int) and
                    isinstance(step, int) and
                    (isinstance(stop, int) or stop is None)):
                raise TypeError("Index for slice in LinkedList.")
            if step <= 0:
                raise ValueError("Step cannot be 0 or less",
                                 " in slice for LinkedList.")
            if stop and start >= stop:
                return res
            
            i = 0
            while i < start:
                if p is None: return res
                p = p.next
                i += 1

            newp = res._head = type(self).Node(p.elem)

            while p and (stop is None or i < stop):
                num = 0 
                while num < step:
                    p = p.next
                    num += 1
                    i += 1
                    if p is None or (stop and i == stop): break
                else:
                    newp.next = type(self).Node(p.elem)
                    newp = newp.next

            return res
        raise TypeError(key, "used as index in LinkedList.")

    def append(self, elem):
        if self._head is None:
            self._head = type(seld).Node(elem)
            return
        p = self._head
        while p.next:
            p = p.next
        p.next = type(self).Node(elem)

    def pop(self):
        if self._head is None: # 空表
            raise LinkedListUnderflow("pop empty LinkedList")
        p = self._head
        if p.next is None:
            e = p.elem
            self._head = None
            return e
        while p.next.next:
            p = p.next
        e = p.next.elem
        p.next = None
        return e

    def prepend(self, elem):
        self._head = type(self).Node(elem, self._head)

    def prepop(self):
        if self._head is None:
            raise LinkedListUnderflow("prepop empty LinkedList")

        e = self._head.elem
        self._head = self._head.next
        return e
     
    # 弄不懂
    def reverse(self):
        p = None
        while self._head:
            q = self._head
            self._head = q.next
            q.next = p
            p = q
        self._head = p

    def mutate_elements(self, trans):
        p = self._head
        while p:
            p.elem = trans(p.elem)
            p = p.next

    def sort(self):
        p = self._head
        if p is None or p.next is None:
            return

        rem = p.next
        p.next = None
        while rem:
            q, p = None, self,_head
            while p and p.elem <= rem.elem:
                q = p
                p = p.next

            if  q is None:
                self._head = rem
            else:
                q.next = rem
            q = rem
            rem = rem.next
            q.next = p

    def __reversed__(self):
        self.reverse()
        try:
            p = self._head
            while p:
                yield p.elem
                p = p.next
        finally:
            self.reverse()

# 带尾节点指针的单链表
class LinkedList1(LinkedList):
    @staticmethod
    def _set_rear(lst):
        p = lst._head
        if p is None: return
        while p.next:
            p = p.next
        lst._rear = p

    def __init__(self, inits=()):
        LinkedList.__init__(self, inits)
        self._set_rear(self)

    def __getitem__(self, key):
        res = LinkedList__getitem__(self, key)
        if isinstance(key, int):
            return res

        # 求值 res,即基类__getitem__,得到链表切片
        # 然后再设置尾节点指针
        self._set_rear(res)
        return res

    def prepend(self, elem):
        if self._head is None:
            self._head = type(self).Node(elem, self._head)
            self._rear = self._head
        else:
            self._head = type(self).Node(elem, self._head)

    def append(self, elem):
        if self._head:
            self._rear.next = type(self).Node(elem)
            self._rear = self._rear.next
        else:
            self._head = self.rear = type(self).Node(elem)

    def pop(self):
        if self._head is None:
            raise LinkedListUnderflow("pop_last empty LinkedList1")
        p = self._head
        if p.next is None:
            e = p.elem
            self._head = None
            self._rear = None
            return e
        while p.next.next:
            p = p.next
        e = p.next.elem

        # 不懂
        p.next = None
        self._rear = p
        return e

    def reverse(self):
        p = self._head
        LinkedList.reverse(self)
        self._rear = p

    def sort(self):
        LinkedList.sort(self)
        self._rear(self)


# 双链表
class DLNode(SLNode):
    __slots__ = ("prev",)
    def __init__(self, elem, prev=None, next_=None):
        SLNode.__init__(self, elem, next_)
        self.prev = prev

class DLinkedList(LinkedList):
    Node = DLNode

    @staticmethod
    def _set_prev_and_rear(lst):
        p = lst._head
        if p is None:
            return
        
        # 还得多想
        p.prev = None
        while p.next:
            p.next.prev = p
            p = p.next
        lst._rear = p

    def __init__(self, inits=()):
        LinkedList.__init__(self, inits)
        self._set_prev_and_rear(self)

    def __getitem__(self, key):
        res = LinkedList.__getitem__(self, key)
        if isinstance(key, int):
            return res
        self._set_prev_and_rear(res)
        return res

    def reverse(self):
        LinkedList.reverse(self)
        self._set_prev_and_rear(self)

    def sort(self):
        LinkedList.sort(self)
        self._set_prev_and_rear(self)

    def prepend(self, elem):
        p = DLNode(elem, None, self._head)
        if self._head is None:
            self._rear = p
        else:
            p.next.prev = p
        self._head = p

    def prepop(self):
        if self._head is None:
            raise LinkedListUnderflow("in prepop empty DLinkedList")
        e = self._head.elem
        self._head = self._head.next
        if self._head:
            self._head.prev = None
        return e

    def append(self, elem):
        p = DLNode(elem, self._rear, None)
        if self._head is None:
            self._head = p
        else:
            p.prev.next = p
        self._rear = p

    def pop(self):
        if self._head is None:
            raise LinkedListUderflow("in pop_last of LDList ???")
        e = self._rear.elem
        self._rear = self.rear.prev
        if self._rear is None:
            self._head = None
        else:
            self._rear.next = None
        return e

    def __reversed__(self):
        p = self._rear
        while p:
            yield p.elem
            p = p.prev


if __name__ == "__main__":

    temp = LinkedList([1, 2, 3])
    # [[1, 2, 3]]
    print(temp)

    # temp._head:  1 <__main__.SLNode object at 0x01450568>
    print("temp._head: ", temp._head.elem, temp._head.next)

    # temp._head.next :  2 <__main__.SLNode object at 0x01450F70>
    print("temp._head.next : ",temp._head.next.elem,temp._head.next.next)

```

## chap5 面向对象编程进阶
元类，用于创建类
#### 1 追踪实例：增加实例计数功能的 *元类*
统计 Rational/Counter 实例引用次数p329
```
class Counter(type):
    def __init__(cls, clsname, bases, attrdict, **kwds):
        init = cls.__init__
        
        def tmp(self, *args, **kwargs):
            init(self, *args, **kwargs) # 调用类的初始化函数
            cls.__objnum += 1
            
        cls.__objnum = 0
        cls.__init__ = tmp
        cls.objnum = classmethod(lambda cls: cls.__objnum)

class Rational(metaclass=Counter):
    "有理数类，取初始化部分，供 Counter 使用"
    
    @staticmethod
    def _gcd(m, n):
        if n == 0:
            m, n = n, m
        while m != 0:
            m, n = n % m, m
        return n
        
    def __init__(self, num, den=1):
        if not (isinstance(num, int) and isinstance(den, int)):
            raise TypeError
        if den == 0:
            raise ZeroDivisionError
        sign = 1
        if num < 0:
            num, sign = -num, -sign
        if den < 0:
            den, sign = -den, -sign
        g = Rational._gcd(num, den)
        self._num = sign * (num // g)
        self._den = den // g

class MRational(Rational, metaclass=Counter):
    pass
a =  MRational(4, 6)
b =  MRational(2)

# <bound method Counter.__init__.<locals>.<lambda> of <class '__main__.MRational'>>
print(MRational.objnum) # MRational.objnum 不加括号结果如上

c = Rational(6, 9)
d = Rational(5)
e = Rational(9, 10)

# 加括号是为函数调用
print(Rational.objnum()) #：5
```

用装饰器实现类似计数功能：实例创建个数p322
```
def add_counter(cls):
    cls.__objnum = 0
    
    init = cls.__init__
    def tmp(self, *args, **kwargs):
        init(self, *args, **kwargs) # 调用类的初始化函数
        cls.__objnum += 1
    
    # 设置删除函数
    # 此时 Rational 类需要增加函数定义：__del__
    # dele = cls.__del__
    # def tmp2(self, *args, **kwargs):
        # dele(self, *args, **kwargs)
        # cls.__objnum -= 1
    # cls.__del__ = tmp2
    
    cls.__init__ = tmp
    cls.objnum = classmethod(lambda cls: cls.__objnum)
    
    return cls

@add_counter
class Rational(): ...
```

但是，以上例子未考虑 *实例删除后* 的计数情况，改进如下：
```
class Counter:
    __objnum = 0
    
    def __init__(self):
        Counter.__objnum += 1
        
    def __del__(self):
        Counter.__objnum -= 1
        
    @classmethod
    def get_objnum(cls):
        return Counter.__objnum
x = Counter()
y = Counter()
z = y
del y
print(Counter.get_objnum()) # 2
```
#### 2 自定义属性字典
`__prepare__` 代替 `dict`<br>
有序字典：维持创建类时属性定义的顺序
```
import collections

# OrderedClass 是元类，其实例就是基于它定义的类
class OrderedClass(type):
    
    @classmethod
    def __prepare__(metacls, name, bases, **kwds):
        # __prepare__ 以特殊方式为类对象准备名字空间字典
        # OrderedDict 映射对象，其键值对维持着加入时的顺序
        return collections.OrderedDict()
        
    def __new__(cls, name, bases, namespace, **kwds):
         result = type.__new__(cls, name, bases, dict(namespace))
         
         # 类中各属性的名字，依属性创建顺序记录
         result.members = tuple(namespace)
         
         return result

class A(metaclass=OrderedClass):
    zero = 0
    def one(self): pass
    def two(self): pass
    three = 3
    
# ('__module__', '__qualname__', 'zero', 'one', 'two', 'three')    
print(A.members) 
```

#### 3 元类和装饰器结合 p332
给类中每个方法增加计时功能。但只在开发调试(__debug__)时计时，实际运行时取消计时
```
def timing():
    pass

class MetaTiming(type):
    def __new__(cls, clsname, bases, attrdict, **kwds):
        if __debug__:
            for attr, val in attrdict.items:
                if callable(val): #
                    attrdict[attr] = timing(val) # 装饰方法
        return typr.__new__(cls, clsname, bases, attrdict)           
         
```

#### 类属性管理和操作 
`__getattribute__, __setattr__, __delattr__`<br>
`__getattr__`<br>
求证：`__name`, 类方法中用双下划线开头屏蔽属性名，以下实例是否是其实现原理
```
class Importance:
    # 检查 _time 值是否符合要求。在数据入口避免错误， 保证对象的完整性p333
    
    def __init__(self):
        # format: (12, 40)
        self._time = None
    
    
    # 这个方法劫持-本类对象的所有属性赋值
    # 一旦在此直接对属性赋值，将导致无穷自递归调用
    # 所以需要调用基类的同名方法
    def __setattr__(self, name, value):
        if name == "_time":
            if (value is None or
                isinstance(value, tuple) and len(value) == 2 and 
                isinstance(value[0], int) and 0 < value[0] < 24 and 
                isinstance(value[1], int) and 0 < value[1] < 60):
                
                super().__setattr__(name, value)
            else:
                raise ValueError
        else:
            super().__setattr__(name, value)
    
    
    # 改进 __setattr__， 屏蔽对象的实际属性名p334
    # x.time = ... 赋值时，实际上是设置 _time 属性
    # 此时，执行取值操作 x.time 则报错，x 无 time 属性
    # 故 __getattr__ 设置取值操作
    
    def __setattr__(self, name, v):
        if name == "time":
            if (v is None or
                isinstance(v, tuple) and len(v) == 2 and 
                isinstance(v[0], int) and 0 < v[0] < 24 and 
                isinstance(v[1], int) and 0 < v[1] < 60):
                
                super().__setattr__("_time", v)
            else:
                raise ValueError
                
        # 若无此处分支条件
        # 则执行 x.a = 3 时程序什么也不做
        else:
            super().__setattr__(name, value)
   
    def __getattr__(self, name):
        if name == "time":
            return self._time
```


## 附加:
#### 1 py2.x 字符编码问题<br>
python内部使用的是unicode编码，而外部却要面对千奇百怪的各种编码，那这些编码是怎么转换成内部的unicode呢？对于中文，可以用的常见编码有utf-8，gbk和gb2312等。只需在代码文件的最前端添加如下：
```
# -*- coding: utf-8 -*-
```
这就是告知python我这个文件里的文本是用utf-8编码的，这样，python就会依照utf-8的编码形式解读其中的字符，然后转换成unicode编码内部处理使用。

不过，如果你在Windows控制台下运行此代码的话，虽然程序是执行了，但屏幕上打印出的却不是哈字。这是由于python编码与控制台编码的不一致造成的。Windows下控制台中的编码使用的是gbk，而在代码中使用的utf-8，python按照utf-8编码打印到gbk编码的控制台下自然就会不一致而不能打印出正确的汉字。
- 解决办法一个是将源代码的编码也改成gbk，也就是代码第一行改成：
```
# -*- coding: gbk -*-
```
- 另一种方法是保持源码文件的utf-8不变，而是在’哈’前面加个u字，也就是:
```
s1=u’哈’
print s1
```
这里的这个u表示将后面跟的字符串以unicode格式存储。python会根据代码第一行标称的utf-8编码识别代码中的汉字’哈’，然后转换成unicode对象。如果我们用type查看一下’哈’的数据类型type(‘哈’)，会得到<type ‘str’>，而type(u’哈’)，则会得到<type ‘unicode’>，也就是在字符前面加u就表明这是一个unicode对象，这个字会以unicode格式存在于内存中，而如果不加u，表明这仅仅是一个使用某种编码的字符串，编码格式取决于python对源码文件编码的识别，这里就是utf-8。

Python在向控制台输出unicode对象的时候会自动根据输出环境的编码进行转换，但如果输出的不是unicode对象而是普通字符串，则会直接按照字符串的编码输出字符串，从而出现上面的现象。
- 使用unicode对象的话，除了这样使用u标记，还可以使用unicode类以及字符串的encode和decode方法。

unicode类的构造函数接受一个字符串参数和一个编码参数，将字符串封装为一个unicode，比如在这里，由于我们用的是utf-8编码，所以unicode中的编码参数使用’utf-8′将字符封装为unicode对象，然后正确输出到控制台：
```
s1=unicode(‘哈’, ‘utf-8′)
print s1
```
ecode是将普通字符串按照参数中的编码格式进行解析，然后生成对应的unicode对象，比如在这里我们代码用的是utf-8，那么把一个字符串转换为unicode就是如下形式：
```
s2=’哈’.decode(‘utf-8′)
```
这时，s2就是一个存储了’哈’字的unicode对象，其实就和unicode(‘哈’, ‘utf-8′)以及u’哈’是相同的。

那么encode正好就是相反的功能，是将一个unicode对象转换为参数中编码格式的普通字符，比如下面代码：
```
s3=unicode(‘哈’, ‘utf-8′).encode(‘utf-8′)
```
s3现在又变回了utf-8的’哈’。<br>
————————————————<br>
版权声明：本文为CSDN博主「craftsman2020」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。<br>
原文链接：https://blog.csdn.net/craftsman2020/article/details/109554069

## 双重装饰器
原文链接：https://blog.csdn.net/xiangxianghehe/article/details/77170585
```
def dec1(func):  
    print("1111")  
    def one():  
        print("2222")  
        func()  
        print("3333")  
    return one  
  
def dec2(func):  
    print("aaaa")  
    def two():  
        print("bbbb")  
        func()  
        print("cccc")  
    return two  
 
@dec1  
@dec2  
def test():  
    print("test test")  
  
test()  
```
输出：
```
aaaa  
1111  
2222  
bbbb  
test test  
cccc  
3333
```
>装饰器的外函数和内函数之间的语句是没有装饰到目标函数上的，而是在装载装饰器时的附加操作。
17~20行是装载装饰器的过程，相当于执行了test=dect1(dect2(test))，此时先执行dect2(test)，结果是输出aaaa、将func指向函数test、并返回函数two，然后执行dect1(two)，结果是输出1111、将func指向函数two、并返回函数one，然后进行赋值，用函数替代了函数名test。 22行则是实际调用被装载的函数，这时实际上执行的是函数one，运行到func()时执行函数two，再运行到func()时执行未修饰的函数test。


## py3.9.4 doc 
#### 3.2. The standard type hierarchy
- numbers.Number
- Sequences
 - Strings:
str.encode() can be used to convert a str to bytes using the given text encoding,<br>
and bytes.decode() can be used to achieve the opposite.
 - Bytes
A bytes object is an immutable array. The items are 8-bit bytes, 
represented by integers in the range 0 <= x < 256. 
Bytes literals (like b'abc') and the built-in bytes() constructor can be used to create bytes objects. 
Also, bytes objects can be decoded to strings via the decode() method.
- Set types
- Mappings
 - Dictionaries 
Changed in version 3.7: Dictionaries did not preserve insertion order in versions of Python before 3.6. In CPython 3.6, insertion order was preserved, but it was considered an implementation detail at that time rather than a language guarantee.
- Callable types
- 
