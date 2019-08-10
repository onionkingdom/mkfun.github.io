# Lua 流程控制语句

| 语句              | 解释                            |
| ----------------- | ------------------------------- |
| 单 if 语句        | 由一个 布尔表达式来作为条件判断 |
| if  ... else 语句 | 与else搭配使用                  |

> 在Lua中 if 的条件表达式可以返回任何值，**false 和 nil 表示假**，**true 和 非nil 表示真**
>
> 所以 Lua 中 ** 0 表示真**

```lua
if(0)
then
  print("true")
end

-- 结果 ： true
```



## 单 if 语句

```lua
if (statements)
then
    code block
end
```



## 与 else 连用

```lua
x , y= 10, 9

if  (x >  y)
then
    print(" x > y")
else
    print("x <= Y")
end
```



## if ... elseif ... else 连用

> 在 lua 中 if 语句可以与 elseif 语句连用用来检测多个条件 , elseif 可以使用多个

```lua
x , y= 10, 9

if ( y > x )
then
    print("y > x")
elseif( y < x )
then
    print("y < x")
else
    print("y = x")
end
```

