# P4P
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
ceil(-0.9) # 0
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

条件表达式 <br>
把 x 的绝对值赋值给 y ： `y = x if x >= 0 else -x`

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
> 例子 自然数指数乘幂: x**n
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
> **循环不变式**证明 while LOOP p29 <br>
> 0~ 如果 每次判断循环条件时 f1 的值总是第 k 个斐波那契数，而 f2 的值总是第 k+1 个斐波那契数， 当 k=n 时循环结束，f1 就一定是第 n 个斐波那契数了。<br>
> 1~ 初始化中，f1==0，也即 F0，且 f2 的值是 F1，是下一个斐波那契数，所需关系成立。<br>
> 2~ 假设某次判断循环条件时，f1 的值是 Fk 且 f2 的值是Fk+1。循环体里的赋值使 f1 f2 分别变成 Fk+1 Fk+2，随后变量 k 的值加1，f1 f2 重新变成 Fk Fk+1。因此在下一次判断循条件时，所需关系仍然成立。<br>
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
        rel = （(self._num * another.den())/
              (self._den * another.sun())）
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
list <br>
    `对象信息 + （） --> 元素存储区（元素的标识符外置）` <br>
    扩容策略：等比扩容， append 分摊式时间复杂度O(1) <br>
    list1.clear() O(n),若触动元素回收机制，开销更大 <br>
    不会自动压缩储存，当大表删除元素小表合算时，只能另建新标 copy 旧表元素 <br>
dict <br>
    `对象信息 + （） --> 元素存储区（元素的标识符外置）` <br>
    hash表效率通常情况下检索是常量时间，特殊情况O(n) <br>
    一般而言 CPython 字典的良好性能是概率意义上的，没绝对保证 <br>
    若程序操作对时间要求特别严格，要考虑其他结构 <br>
