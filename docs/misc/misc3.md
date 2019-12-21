使用 YAML 来保存你的游戏数据 ！

为什么是 YAML ？

首先我们来看看什么是YAML ：

![image-20191128121202614.png](https://i.loli.net/2019/12/21/37jolPuerIWg4Yb.png)

- YAML 不是标记语言。
- YAML 是针对所有编程语言的人性化数据序列化标准。
- 像 XML 一样，它使用可移植的、独立于平台的格式来表示任何种类的的数据，但是它是人性化的这意味着它更方便我们阅读。
- 同时 YAML 也是 Unity 编辑器使用的序列化格式。

它看起来像是这样 ：

    boolean: 
    	- TRUE
    	- FALSE
    float:
    	- 3.14
    	- 3.13e+5
    int:
    	- 123
    null:
    	nodeName: 'node'
    string:
    	- 'Hello world'
    date:
    	- 2020-01-01
    datetime:
    	- 2020-01-01T15:01:01+08:00

YAML 的基本语法

- 大小写敏感。
- 使用缩进来表示层级关系。
- 缩进不允许使用 Tab
- 使用 # 来表示单行注释，不支持多行注释。
- 文件开头要使用 --- 来表示文档开始，使用 ... 来表示。文档结束，一个文件中可以存在多个文档。
- 文件的拓展名一般为 .yaml 比如 Player.yaml

YAML 支持的数据结构

翻译并不完全准确，每个人译法不同请以英文为主。

- 散列表 (  mappings   )
- 数组 (  sequences )
- 纯量 (  scalars )

现在我们来学习一下三大数据结构，这一部分内容请结合下方的特殊符号讲解一起看。

1 .散列表 ： 

    ---
    # 使用冒号来代表，格式为 key: value 冒号后面要加一个空格
    key: value
    
    # 使用缩进来表示层级关系
    key:
        key1: value1
        key2: value2
    
    # flow 风格写法
    key: {key1: value1, key2: value2}
    
    # 无序键值对
    map:
      Block style: !!map
        key1 : value1
        key2  : value2
     # Flow style
     Flow style: !!map { key1: value1, key2: value2 }
     
     # 结果
     # map: 
     #   { 'Block style': { key1: 'value1', key2: 'value2' },
     #     'Flow style': { key1: 'value1', key2: 'value2' } }
     
     # 有序键值对 （字典）
     omap:
      Block style: !!omap
        - one: 1
        - two: 2
        - three: 3
      # Flow style
      Flow style: !!omap [ one: 1, two: 2, three : 3 ]
    # 结果
    # omap: 
    #    { 'Block style': [ { one: 1 }, { two: 2 }, { three: 3 } ],
    #      'Flow style': [ { one: 1 }, { two: 2 }, { three: 3 } ] }
    ...

2 .数组

    ---
    # 普通定义
    食物 :
     - 胡萝卜
     - 西红柿
     - 苹果
     # 结果 : { '食物': [ '胡萝卜', '西红柿', '苹果' ] }
    
    # 嵌套键值对
    食物 : 
     - 蔬菜: 胡萝卜
     - 蔬菜: 西红柿
     - 水果: 苹果
    # 结果：
    # { '食物': [ { '蔬菜': '胡萝卜' }, { '蔬菜': '西红柿' }, { '水果': '苹果' } ] }
    
    # pairs 类型
    食物 : !!pairs
     - 蔬菜: 胡萝卜
     - 蔬菜: 西红柿
     - 水果: 苹果
    # 结果 : { '食物': [ [ '蔬菜', '胡萝卜' ], [ '蔬菜', '西红柿' ], [ '水果', '苹果' ] ] }
    ...

3 .纯量 （ 不保证每个解析器都能正常使用所有类型，请自行实际判断 ）

1 .Integers 整型

    ---
    # Integers 整型
    canonical: 12345   # 普通 int
    decimal: +0.12345  # 小数
    octal: 014         # 8 进制
    hexadecimal: 0xC   # 16 进制
    
    # 结果均为 10 进制 ：
    # { canonical: 12345,
    #   decimal: 12345,
    #   octal: 12,
    #   hexadecimal: 12 }
    # 注：请注意最终结果与冒号前的名字并无关系，与冒号后的写法有关，不要混淆
    ...

2 .Floating Point 浮点数

    ---
    # Floating Point 浮点数
    canonical: 1.23015e+3     # 普通 float
    exponential: 12.3015e+02  # 科学计数法
    fixed: 1230.15            # 固定值
    negative infinity: -.inf  # 负无穷大
    not a number: .NaN        # 不是数字
    ...

3 .  Timestamps 时间

    ---
    canonical: 2001-12-15T02:59:43.1Z
    iso8601: 2001-12-14t21:59:43.10-05
    spaced: 2001-12-14 21:59:43.10 -5
    date: 2002-12-14
    
    # 结果
    # { canonical: Sat Dec 15 2001 10:59:43 GMT+0800 (中国标准时间),
    #   iso8601: Sat Dec 15 2001 10:59:43 GMT+0800 (中国标准时间),
    #   spaced: Sat Dec 15 2001 10:59:43 GMT+0800 (中国标准时间),
    #   date: Sat Dec 14 2002 08:00:00 GMT+0800 (中国标准时间) }
    ...

4 . 其它常用类型

    ---
    
    # 布尔类型
    boolean:
      - true
      - false
    # 结果 ：{ bool: [ true, false ] }
    
    # 字符串类型
    string : '123456'
    # 结果 ：{ string: '123456' }
    
    # 空值
    null :
     - ~
     - null
    # 结果 ：{ null: [ null, null ] }
    ...

现在我们来看看 YAML 中的特殊符号：

1 . " --- " 和 " ... "

    # --- 代表一个文档的开始
    --- 
    
    # !! 俩个感叹号 用来做强制类型转换
    test : 
     - !!int 123
     - !!int 123
    # 结果 ： { test: [ 123, '123' ] } 可以看到第一个为整数类型，第二个为字符串类型。
    
    ...
    # ... 代表一个文档的结束

2 . “ > ”  和 " | "

    ---
    # 在字符串中 “>” 大于号表示换行，“|” 竖线同样表示换行但是保留换行符
    test1 : >
     这是一段
     文字。
    test2 : |
     这是一段
     文字。 
    # 结果 ： { test1: '这是一段 文字。\n', test2: '这是一段\n文字。\n' }
    ...

3 . " ? " 和 " : "

    ---
    # 对于复杂的对象格式可以使用 ？ 加空格来代表 key ； 使用 ： 加空格来代表 value
    ? 
     - complexkey
     - complexkey2
    : 
     - value1
     - value2
    
    # flow 风格写法
    [complexkey,complexkey2] : [value1,value2]
    
    # 结果 ：{ 'complexkey,complexkey2': [ 'value1', 'value2' ] }
    ...

4 . " & " 、" << " 和 " * "

    ---
    # 引用重复的内容 “&”：锚点标记、“<<”: 合并标记、“*”：要合并的锚点值
    # 要注意空格的数量
    - test: &001
       key1 : value1
       key2 : value2
    - test1: 
       <<: *001
       key2 : value222  # 重写 key2
    - test2:
       <<: *001
       key3 : value3 # 添加 key3
    # 结果 ：
    # [ { test: { key1: 'value1', key2: 'value2' } },
    #   { test1: { key1: 'value1', key2: 'value222' } },
    #   { test2: { key1: 'value1', key2: 'value2', key3: 'value3' } } ]
    
    # 一些简单的合并也可以不使用 << 比如
    sex:
     - &00 male
     - &01 female
    player1:
      - sex : *00
    player2:
      - sex : *01
    # 结果：
    # { sex: [ 'male', 'female' ],
    #   player1: [ { sex: 'male' } ],
    #   player2: [ { sex: 'female' } ] }
    ...

在 Unity 中使用 YAML ！

  YAML 在很多语言中都可以方便的使用，unity 中也不例外。

1 .在资源商店中查找 YamlDotNet for Unity 这是一个免费的插件，将它导入到你的项目中。

2 .导入后你的项目中应该是这样的，在 Plugin 文件夹下 多出一个名为 YamlDotNet 的文件夹。

![image-20191128203306266.png](https://i.loli.net/2019/12/21/xk5AmELPQbtDpud.png)

3 .创建一个脚本来测试下我们的功能，创建一个玩家类来保存信息。

    // 创建一个玩家类用来保存玩家信息
    internal class PlayerData
    {
        public string PlayerName { get; set; }
        public string PlayerSex { get; set; }
        public List<int> PlayerBackPack { get; set; }
    }

4 .简单的存储读取

    using UnityEngine;
    using System.Collections.Generic;
    using YamlDotNet.Serialization;
    
    public class YamlTest : MonoBehaviour
    {
        private void Start()
        {
            //创建对象
            var data = new PlayerData 
            {
                PlayerName = "SuperSoda",
                PlayerSex = "男",
                PlayerBackPack = new List<int>()
                {
                    1, 2, 3, 4, 5
                }
            };
            
            //序列化为 YAML 
            var serializer = new Serializer();
            var yaml = serializer.Serialize(data);
            Debug.LogFormat("序列化保存:\n{0}", yaml);
            
            //反序列化
            var deserializer = new Deserializer();
            var data1 = deserializer.Deserialize<PlayerData>(yaml);
            Debug.Log("反序列化读取:");
            Debug.Log("玩家名字 : " + data1.PlayerName);
            Debug.Log("玩家性别 : " + data1.PlayerSex);
            Debug.Log("玩家物品总数 : " + data1.PlayerBackPack.Count);
            Debug.Log("背包中第一个物品id : " + data1.PlayerBackPack[0]);
        }
    }

5 .运行看看最终效果吧 ~ 😎

![image-20191128213828612.png](https://i.loli.net/2019/12/21/F4wtaIHQKl5PAMr.png)

大功告成 ！，我们再来看看 YAML 中这一段的样子：

    # 源代码简洁明了，方便更改
    PlayerName: SuperSoda
    PlayerSex: 男
    PlayerBackPack:
     - 1
     - 2
     - 3
     - 4
     - 5
     
     # 格式化后是这样
     # { 
     #  PlayerName: 'SuperSoda',
     #  PlayerSex: '男',
     #  PlayerBackPack: [ 1, 2, 3, 4, 5 ] 
     # }

🤠 上述内容希望对大家有帮助，同时欢迎纠错 ~

参考

 https://yaml.org/spec/1.2/spec.html 

 https://www.jianshu.com/p/97222440cd08 

 http://www.ruanyifeng.com/blog/2016/07/yaml.html 
