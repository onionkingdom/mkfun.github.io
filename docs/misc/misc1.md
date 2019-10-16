# C#中 ==、Equals、ReferenceEquals 对比解析

## 1. 相等运算符

> 对于 “!=” 不等于运算符，**x != y** , 与 **!(x ==y )** 相同

操作数相同，等于运算符返回 true ， 否则返回 false 。如果内置值类型的值相等，则其操作数相等。如果基本整数类型的相应值类型相等，则相同美剧类型的俩个操作数相等。

``` C#
int a = 3 + 3;
int b = 6;
Console.WriteLine(a == b); //output : True

char c1 = 'a';
char c2 = 'A';
Console.WriteLine(c1 == c2);  // output: False
Console.WriteLine(c1 == char.ToLower(c2));  // output: True
```

对于引用类型，如果俩个引用类型用了同一个对象，则这俩个炒作符相等

```C#
public class MyClass
{
    private int id;
    
    public MyClass(int id) => this.id = id;
}

public static void Main()
{
    var a = new MyClass(1);
    var b = new MyClass(1);
    var c = a;
    Comsole.WriteLine(a == b); //output False
    Console.WriteLine(c == a); //output True
}
```

对于字符串来说，如果俩个字符串均为 NULL 或者俩个字符串的实例有相等的长度同时每个字符位的字符相同，这俩个字符串的操作数相等。

```C#
string s1 = "Hello";
string s2 = "Hello";
string s3 = "hello";

Console.WriteLine(s1 == s2); //output True
Console.WriteLine(s2 == s3); //output False
```

对于委托来说，如果俩个委托都是 NULL 或者它们的调用列表长度相同而且在每个位置有相同的条目，这时它们的操作数是一样的。

```c#
Action a = () => Console.WriteLine("a");

Action b = a + a;
Action c = a + a;
Console.WriteLine(b == c); //output True

//对于通过语义上相同的 Lambda 表达式生成的委托，它们并不相等.

Action a = () => Console.WriteLine("a");
Action b = () => Console.WriteLine("a");

Console.WriteLine(a == b);  // output: False
Console.WriteLine(a + b == a + b);  // output: True
Console.WriteLine(b + a == a + b);  // output: False
```

## 2. Equals

equals() 方法用来比较俩个对象的内容是否一致，也就是比较引用值类型是否是同一个对象的引用。 equals() 的静态方法带有俩个参数，用于处理其中一个值为 null 的情况，因此如果有俩个对象如果其中一个可能为 null 时使用静态方法。静态方法调用后如果俩个值都不为空那么他将会调用虚拟方法，所以如果重写Equals()的实例版本，它的效果相当于也重写了静态版本。

```C#
int a = 5;
int b = 5;

// 或者使用 if(a.Equals(b))
If(Object.Equals(a ,b))
{
	Console.WriteLine(“a is equal to b”);
}
```



## 3. ReferenceEquals

ReferenceEquals 是Object 的静态方法，用于比较俩个引用类型的对象是否是同一个对象的引用 ，即俩个引用是否包含内存中相同的地址如果提供的俩个引用指向同一个对象的实例返回 true，**要注意的是它认为 null 等于 null，对于值类型他总是返回 false**。

```C#
SomeClass x, y;

x = new SomeClass();
y = new SomeClass();

bool B1 = ReferenceEquals(null, null);//output True
bool B2 = ReferenceEquals(null, x);//output False
bool B3 = ReferenceEquals(x, y); //output False
```

## 总结

在 C# 中只用俩类比较：

1. 值类型比较 ：判断俩个值类型的实例的值是否相等。

2. 引用类型比较：

   1. 引用类型的引用 （存在于线程栈中）
   2. 引用类型的值 （存在于托管堆中）

   