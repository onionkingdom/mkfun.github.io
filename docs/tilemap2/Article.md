# 上文中的不足

1. 只有一个 Tilemap，不方便后续功能添加
2. 地图中小凸起较多 (毛刺)，不方便用玩家行走
3. 地图生成没有控制系统，不利于地图分享保存

今天就开始优化上面三个问题，以下是优化方案 ：

1. 再添加一个 Tilemap ，将 **地板** 和 **围墙** 分开，利于后期分层进行操作
2. 添加一个剔除方法，如果一个墙壁四周均为地板，那么将他删除掉
3. 添加一个 **种子** 然后根据种子来生成地图，这样就可以方便分享地图



![完成截图.png](https://i.loli.net/2019/08/12/KBCIgiMROav2tdJ.png)

完成后的所有功能截图

## 代码实现

### 新建变量

- floor map （存放地板）
- wall map （存放墙壁）
- end tile （存放最后一个生成的 Tile，用来传送到下一个关卡）
- cutting （是否开启地图裁剪）
- map seed （存放地图种子，用来生成地图）

``` c#
public Tilemap floorMap;
    public Tilemap wallMap;
    public TileBase wallTile;
    public TileBase floorTile;
    public TileBase endTile;
    public bool cutting; //是否剔除
    public int mapSeed;  //地图种子
    public int step;

    Dictionary<int,Vector3Int> _road = new Dictionary<int,Vector3Int>(); //生成路的坐标
    List<Vector3Int> _wall = new List<Vector3Int>();//墙的坐标
```

### 地板的生成 

​	地板的坐标用字典来表示，key 表示地板类型（传送门 、普通地板...）value 来表示地板的坐标

``` C#
//生成地板
    private void BuildRoad()
    {
        _wall.Clear();
        _road.Clear();
        
        floorMap.ClearAllTiles();
        wallMap.ClearAllTiles();
        //生成起点
        var nowPoint = new Vector3Int(0, 0, 0);

        //按指定步数生成路径
        for(int i = 0; _road.Count < step;)
        {
            if (!(_road.ContainsValue(nowPoint)))
            {
                _road.Add(i,nowPoint);
                i++;
            }
            nowPoint += RandomRoadPoint();
        }
    }
```
### 墙壁的生成
​	墙壁的坐标用 List 来表示，生成墙壁的时候为了视觉上更加美观，避免出现角落缺块的情况，我多加了一些额外的判断。

![角落缺块.png](https://i.loli.net/2019/08/12/wg24BJTvsnHlUWd.png)

```C#
//生成墙壁
    private void BuildWall()
    {
        foreach (var item in _road.Values)
        {

            var right = item + Vector3Int.right;
            var left = item + Vector3Int.left;
            var up = item + Vector3Int.up;
            var down = item + Vector3Int.down;

            var topleft = item + Vector3Int.up + Vector3Int.left;
            var topright = item + Vector3Int.up + Vector3Int.right;
            var bottomleft = item + Vector3Int.down + Vector3Int.left;
            var bottomright = item + Vector3Int.down + Vector3Int.right;


            if (!(_wall.Contains(up))    && !(_road.ContainsValue(up))) { _wall.Add(up); }
            if (!(_wall.Contains(down))  && !(_road.ContainsValue(down))) { _wall.Add(down); }
            if (!(_wall.Contains(left))  && !(_road.ContainsValue(left))) { _wall.Add(left); }
            if (!(_wall.Contains(right)) && !(_road.ContainsValue(right))) { _wall.Add(right); }
            if (!(_wall.Contains(topleft)) && !(_road.ContainsValue(topleft))) { _wall.Add(topleft); }
            if (!(_wall.Contains(topright)) && !(_road.ContainsValue(topright))) { _wall.Add(topright); }
            if (!(_wall.Contains(bottomleft)) && !(_road.ContainsValue(bottomleft))) { _wall.Add(bottomleft); }
            if (!(_wall.Contains(bottomright)) && !(_road.ContainsValue(bottomright))) { _wall.Add(bottomright); }

        }
    }
```
### 终点格子的生成

​	现在我们的墙壁和地板已经是分开俩个 map 去实现了，在真实情况下我们肯定是需要一个进入下一个关卡的入口的，为了方便实现同时增加用户的探索时长。我直接把最后一个生成的地板替换为了终点前往下一关的传送门。（这样并不是很完美，未来可能会加入钥匙🔑机制）

``` C#
//生成终点格子
    private void CreateDestination()
    {
        if (_road.Count > 0)
        {
            floorMap.SetTile(_road[(step-1)],endTile);
        }
    }
```
### 地图剔除“毛刺”实现
​	文章开头就说过，这个地图生成后会有一些“小毛刺”墙壁，所以我们要实现一个方法去优化这个问题，对比图如下 ：

![开启优化.png](https://i.loli.net/2019/08/12/akKGVR6cvzXAUwJ.png)

开启优化

![关闭优化.png](https://i.loli.net/2019/08/12/Wr4BCIy6L2tMhiw.png)

关闭优化

​	可以明显看到我们的地图更加开阔，玩家的操作体验额有了提升。
``` C#
//修剪地图
    private void CutMap()
    {
        var tempWall = new List<Vector3Int>(_wall);
        var countRoad = 0;
        int key = -1; //作为新加入的的tile的key
        foreach (var item in _wall)
        {
            var right = item + Vector3Int.right;
            var left = item + Vector3Int.left;
            var up = item + Vector3Int.up;
            var down = item + Vector3Int.down;

            if (_road.ContainsValue(right))
                countRoad++;
            if (_road.ContainsValue(left))
                countRoad++;
            if (_road.ContainsValue(up))
                countRoad++;
            if (_road.ContainsValue(down))
                countRoad++;

            if (countRoad >= 3)
            {
                _road.Add(key,item);
                tempWall.Remove(item);
                countRoad = 0;
                key--;
                continue;
            }

            countRoad = 0;
        }
        _wall = tempWall;
    }
```

​	然后在 Update 函数中检测我们是否开启了修建功能

```C#
private void Update()
    {
    	//为了方便测试我这里设置了按下 R 键生成一次地图
        if (Input.GetKeyDown(KeyCode.R))
        {
            BuildRoad();
            BuildWall();
            if (cutting)
            {
                CutMap();
            }
        }
    }
```



###  实现地图种子

​	我们知道在几乎所有编程语言中，可以使用 相同的**随机数种子** 来生成一系列相同伪随机数。利用这个特点，我们可以在地图生成方法开始前设定我们的随机数种子这样只要种子相同即使是在不同的电脑上我们也可以得到相同的关卡。

```C#
private void Start()
    {
        //设置随机数种子确保地图生成是同样的
       Random.InitState(mapSeed);
    }
```

------



<center>这就是本章的所有内容啦~</center>

![牛.jpg](https://i.loli.net/2019/08/08/J1vgVLsZ4fxPMaX.jpg)

