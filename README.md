# p4p 
程序员学Python笔记

## chap1 
### 算数运算
取余 
整点数和浮点数都能取余（p2）
规则 
余数(r) = 被除数(a) - n*(a // n)
例子 <br>
···
-7 = 123 % （-10）
   = 123 - (-10)(123 // -10) 
   = 123 - (-10)(-13) # // 向下取整
 
 7 = -123 % 10
   = -123 - 10（-123 // (-10)）
   = -123 - 10*12
  
 -3 = -123 % (-10)
···
#### Python取整——向上取整、向下取整、四舍五入取整、向0取整
向上取整: math.ceil()
    import math.ceil
    ceil(-0.5) # 0
    ceil(-0.9) # 0
    ceil(0.3) # 1
向下取整: floor()
    import.floor
    floor(-0.3) # -1
    floor(0.9) # 0
四舍五入取整: round()
    round(-2,5) 
    round(-1.5)

向0取整: int()
    int(-0.9) # 0
    int(0.5) # 0

取整运算: //(向下取整)

