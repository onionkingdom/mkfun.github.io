# Unity 结构以及代码规范

## 项目文件规范

### 文件夹

使用帕斯卡命名法，文件夹名字不要太长如果实在没有办法简短文件夹名字可以拆为子文件夹。

### 目录、文件结构

```
Root
+---Assets
+---Build
\---Tools           # 程序开发时的辅助工具
```

### 资产

```
Assets
+---Art
|   +---Materials
|   +---Models      # FBX 和 BLEND 文件
|   +---Textures    # PNG 文件
+---Audio
|   +---Music
|   \---Sound       # 采样 音效
+---Code
|   +---Scripts     # C# 脚本
|   \---Shaders     # Shader 文件 以及 shader graphs
+---Docs            # Wiki, 概念图, 宣传资料
+---Level           # 与 Unity 游戏设计有关的内容
|   +---Prefabs
|   +---Scenes
|   \---UI
\---Resources       # 配置文件, 本地化文本和其他用户文件。
```

**另外一种**

```
Assets
+---Art
|   +---Materials
|   +---Models      # FBX 和 BLEND 文件
|   +---Music
|   +---Prefabs
|   +---Sound       # 采样 以及 音效
|   +---Textures
|   +---UI
+---Levels          # Unity 场景文件
+---Src             # C# 脚本 和 shaders
|   +---Framework
|   \---Shaders
```

### 脚本

1. 使用与目录结构相匹配的命名空间
2. Framework 目录非常适合存放可以跨项目重用的代码
3. Scripts 文件夹根据项目的不同而不同，但是  `Environment`, `Framework`, `Tools` 和`UI` 应该保持一致

```
Scripts
+---Environment
+---Framework
+---NPC
+---Player
+---Tools
\---UI
```

### 模型文件

```
Models
+---Blend
\---FBX
```

# 工作流程

### 模型

使用 FBX 格式的模型文件

虽然 Unity 原生支持 Blender 文件，但是最好将正在处理的和已经导出的文件分开存放。同样使用其它软件时也是如此，比如使用 Substance 制作的纹理。

使 Y 轴为 up 方向，-Z 为 forward 方向， 以及使用等比的缩放。

### 贴图

使用 PNG，TIFF 或 HDR

 选着`Specularity/Glossiness` 或 `Roughness/Metallic` 工作流. 



| 后缀 | 材质                           |
| ---- | ------------------------------ |
| _AL  | Albedo (反射)                  |
| _SP  | Specular (高光)                |
| _R   | Roughness (粗糙度)             |
| _MT  | Metallic (金属)                |
| _GL  | Glossiness (光泽度)            |
| _N   | Normal (普通)                  |
| _H   | Height (高度)                  |
| _DP  | Displacement (位移)            |
| _EM  | Emission (放射光)              |
| _AO  | Ambient Occlusion (环境光遮罩) |
| _M   | Mask ( [ 遮挡 ])               |

### RGB 遮罩

>不是美术人员，不太懂这一块

 优良作法是使用单个纹理在每个RGB通道拆分的单个纹理中组合黑白蒙版。 

``` 
texture_AL.png  # Albedo
texture_N.png   # Normal Map
texture_M.png   # Mask
```

| Channel | Spec/Gloss        | Rough/Metal       |
| ------- | ----------------- | ----------------- |
| R       | Secularity        | Roughness         |
| G       | Glossiness        | Metallic          |
| B       | Ambient Occlusion | Ambient Occlusion |

### 配置文件

使用 ini 格式保存，快速且解析简单，直观且易于调整

XML 、JSON 、 YAML 也是不错的选择，对于玩家不能更改的文件使用二进制格式，多人联机游戏的数据必须放在服务器。

### 本地化

使用 CSV 文件 

本地化软件常常使用的一中格式，可以轻松的通过电子表格读取字符串。

### 音频

混合时使用 WAV 格式，在游戏中使用 OGG格式

预先加载较小的音频片段到内存中，实时加载更长的音乐和较少的环境声文件。

### 参考

[UnityStyleGuide]( https://github.com/stillwwater/UnityStyleGuide )
