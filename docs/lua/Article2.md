# Lua 循环

| 循环类型         | 循环条件           |
| ---------------- | ------------------ |
| while 循环       | 条件为 **真** 循环 |
| for 循环         | 在语句块中控制条件 |
| repeat ... until | 条件为 **假** 循环 |

> Lua 中没有 continue 语句 ，只有break。

## while 循环

``` lua
while(condition)
do
    statemen
end
```

**循环体语句(statements)** 可以是一条，或者一个代码块

**条件(conditon)** 可以是 布尔值 ，或者任意表达式语句

在条件为 true 时执行循环

## for循环

``` lua
for var = exp1, exp2, exp3 do
    <code block>
end
```

- 变量 var 初始值 为 exp1
- 一直变化到 exp2
- 步长为 exp3
- 每次变化 执行一次 代码块内容

### 泛型 for 循环

类似于 foreach ，通过一个迭代器来实现遍历所有值

``` lua
-- 打印所有值
x = {"a", "b", "c"}
for index, value in ipairs(x) 
do
    print (index, value)
end
-- 结果
-- 1 a 
-- 2 b
-- 3 c
```

- index 是数组的索引值
- value 是索引对应的值
- ipairs 是Lua提供的一个 迭代器函数，用来迭代数组

> Lua 中 pairs 和 ipairs 的区别

- 迭代  table 用 pairs
- 迭代  array 用 ipairs
- ipairs 从下标为 1 开始遍历，然后下标累加 1，如果某个下标元素不存在就终止遍历。这就导致了如果下标不连续或者是不是从 1 开始的表就会中断或者是遍历不到元素

``` lua
x = {[1] ="a", [2] = "b",[3] =  "c",[5] =  "d", [6] = "e"}

for k, v in pairs(x) 
do
    print(k,v)
end

print ("====")

for v,k in ipairs(x)
do
    print(v,k)
end

-- 运行结果 
-- 1    a
-- 2    b
-- 3    c
-- 5    d
-- 6    e
-- ====
-- 1	a
-- 2	b
-- 3	c
```

对比结果，可以看出ipairs如果遇到 nil 就会中断，而且如果第一个元素的下标不是 1 就会漏掉第一个元素，所以遍历table一般用 pairs

# repeat  ... until 循环

>   这个循环 和 上俩个不同，for 和 while 循环的条件判断语句是在第一次循环开始前进行，而repeat .. until 循环的条件判断语句是在第一次循环结束后判断。（在当前循环结束后判断）

基本语法 ：

``` lua
repeat
    statements
until ( cindition )
```

判断语句在末尾，所以在条件判断前会先执行循环，如果条件为 **false** 循环会重新开始执行，直到条件为 **true** 才会停止 （与while相反）

``` lua
var = 1
repeat
    print(var)
    var = var + 1
until(var > 3)
-- 结果
-- 1 2 3
```

# break 语句

  和其他语言类似，在lua中也有 break 语句，用来退出当前循环或语句，并执行紧接着的语句。在嵌套循环中，break会停止最内层的循环，开始执行外层的循环。

``` lua
x = 1

while (x < 5)
do
    print(x)
    x = x + 1
    if (x > 3)
    then
        break
    end
end

-- 结果 1 2 3
```

可以看出来单层循环会直接跳出。

