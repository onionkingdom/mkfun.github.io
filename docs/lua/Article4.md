# Lua 的函数

  像其他所有编程语言一样，在 Lua 中也有函数这一概念。 函数是 Lua 中语句和表达式抽象的主要机制，可以完成制定的任务，也可以执行计算和返回值。

``` lua
--  Lua 中函数的定义格式如下
optional_function_scope function function_name( argument1, argument2, argument3..., argumentn)
    function_body
    return result_params_comma_separated
end
```

- **optional_function_scope:** 该参数是可选的制定函数是全局函数还是局部函数，未设置该参数默认为全局函数，如果你需要设置函数为局部函数需要使用关键字 **local**。
- **function_name:** 指定函数名称。
- **argument1, argument2, argument3..., argumentn:** 函数参数，多个参数以逗号隔开，函数也可以不带参数。
- **function_body:** 函数体，函数中需要执行的代码语句块。
- **result_params_comma_separated:** 函数返回值，Lua语言函数可以返回多个值，每个值以逗号隔开。

``` lua
-- 比较数字大小
function myFunction ( num1, num2 )
    
    if (num1 < num2) 
    then
        print(num1 .. "<" .. num2)
    elseif(num1 > num2)
    then
        print(num1 .. ">" .. num2)
    else
        print(num1 .. "=" .. num2)
    end

end
-- 调用函数
myFunction(1,2)

-- 结果 ： 1 < 2 
```

> 在 lun 中字符串拼接用的是  ..  要注意和其他语言的区别

## 多返回值函数

在 Lua 中函数可以返回多个值

``` lua
function myFunction()
    return "hello","Lua"
end
print(myFunction())

-- 结果 hello lua
```

## 可变参数

在Lua中使用 ... 来代表函数有可变的参数

```lua
function average(...)
   result = 0
   local arg={...}  
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. #arg .. " 个数")
   return result/#arg
end

print("平均值为",average(10,5,3,4,5,6))
```



