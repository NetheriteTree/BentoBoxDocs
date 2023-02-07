# 常见问题

## 插件安装

### 如何正确安装 BentoBox, BSkyBlock 还有其它种种扩展?

最简单的方式便是从 [https://download.bentobox.world](https://download.bentobox.world) 下载预配包.
你也可以参考[这个教程](BentoBox/Install-Bentobox.md)来了解其它安装方式.
**欢迎加入 BentoBox 大家庭!**

## 插件配置

### 如何创建我自己的岛屿地形模板?

你将用到 BentoBox 的**一个模板格式**叫做**_蓝图_**.
[蓝图帮助页面](BentoBox/Blueprints.md) 介绍了所有有关蓝图的东西, 以及进一步个性化蓝图的提示和技巧.
你也可以参考[这个视频](https://www.bilibili.com/video/BV15T4y1G77Q)提供的创建蓝图的方法(尽管视频已经过时, 但它仍然可以让你在几分钟内学会).

### 支持哪些数据库软件?

数据库最低要求(选择其一即可):

* **MySQL** 5.7 或更高版本
* **MariaDB** 10.2.3 或更高版本
* **MongoDB** 3.6 或更高版本
* **SQLite** 3.28 或更高版本
* **PosgreSQL** 尽量使用最新版

### 如何扩大某个玩家的岛屿大小?

每个岛屿都有一定大小的保护区域. 你最多可以把保护区域扩大至 inter-island 距离. 岛屿范围可以通过指令或权限来更改. 权限只会在玩家登录时检查, 因此当你通过权限控制岛屿大小时, 该玩家必须重新登录来使之生效. 指令则会立即生效. 记住, 保护范围会覆盖整个岛屿.

**权限**
给予岛主 `[游戏模式].island.range.<大小>` 权限.

* 岛主必须重新登录来使之生效
* 如果岛主改变, 那么岛屿范围会根据新岛主的权限而调整, 或者恢复至初始大小(如果新岛主没有相关权限的话).

**指令**

使用 `/[管理员指令前缀] range` 指令.

## 已知问题

### 世界中生成了超平坦区块

*相关问题:*
[BentoBox#1212](https://github.com/BentoBoxWorld/BentoBox/issues/1232),
[BSkyBlock#247](https://github.com/BentoBoxWorld/BSkyBlock/issues/247).

![超平坦世界](https://p1.meituan.net/dpplatform/aea97fed123042a2ba18b838360b775a125356.jpg)
*超平坦类型的世界. (来源: [1213videogamer on PlanetMinecraft](https://www.planetminecraft.com/member/1213videogamer/)).*

如果你发现岛屿世界开始生成超平坦区块, 那么世界生成器可能没有正常工作.
有多种因素会造成此情况. 以下按照可能性从高到低排序.

**我们强烈建议你回档到离此情况发生时最近的一次备份**.
尽管这里也提供了没有备份情况下的解决方案, 但我们**不保证其可用性**. 另外, 那些方案**用于尽可能地定位到问题根源, 但忽略了对服务器性能和玩家岛屿的影响**. 请自行斟酌.

一个快速修复的方法是使用管理员控制面板中的选项来移除超平坦区块. 这是修复此问题的主要工具, 但在你找到问题根源之前, 只会有更多超平坦区块生成并造成更大的危害.

不管怎样, **立即关闭服务器以阻止对世界数据进一步的破坏**.

#### 原因

##### BentoBox 或者某个游戏模式扩展未正常运行

**为什么?**

BentoBox 或其游戏模式扩展并未启用.
这可能是因为你升级了 BentoBox 或是游戏模式扩展, 而新版本的插件或扩展与你的服务器版本不兼容或与其它插件冲突.

**解决方案**

调查 BentoBox 或是游戏模式扩展未启用的原因.
在服务器启动日志中查找报错信息.
尝试一个一个加入其它的插件来排查插件冲突问题.

##### `bukkit.yml` 文件中未指定该世界的生成器

**为什么?**

这是多数发生的情况.
当你把服务器默认世界设置为游戏模式世界时, 你忘了在 `bukkit.yml` 文件中指定正确的生成器.

**解决方案**

确保你从头到尾都是按照[此教程](BentoBox/Set-a-BentoBox-world-as-the-server-default-world.md)来做的.

##### 游戏模式扩展配置文件中的 `use-own-generator` 选项设为了 `true`

**为什么?**

这是个常见的错误.

很多服主会误解这个选项为启用一个“魔法”圆石生成器 (但实际上[它是一个扩展](addons/MagicCobblestoneGenerator/index.md)!).
这根本不是这个选项的功能, 并且注释中写的也很清楚:

```yaml
# 对此世界使用你自己的世界生成器.
# 这种情况下, BentoBox 将不会生成任何东西.
# 如果启用, 你必须在 bukkit.yml 文件中指定该世界的生成器名称.
# 详见 https://bukkit.gamepedia.com/Bukkit.yml
use-own-generator: false
```

当然，此情况也可能是你忘了在 `bukkit.yml` 文件中指定世界生成器.

**解决方案**

如果你并不打算使用其它插件来生成世界, 你应该把这个选项设为 `false`.

否则, 你应该确保你在 `bukkit.yml` 文件中指定了正确的生成器名称.

##### 另一个插件试图控制此世界的生成

**为什么?**

尽管很少见, 这仍然可能发生.

部分插件, 特别是世界管理插件 (比如 Multiverse), 通常会提供可以覆盖世界生成器的选项.

**解决方案**

浏览你的插件列表确定最可能造成此问题的插件.
世界管理插件和与世界生成有关的自制插件应该首先被检查.
向该插件开发者汇报问题或是修改配置文件为正确的值.  

##### 你发现了 BentoBox 或其扩展的 Bug

**为什么?**

*Woopsie!*

现在，这几乎不会出现.
但由于某些原因，这仍然可能出现.

**解决方案**

确保这是 BentoBox 相关的 Bug: 一个一个移除你服务器的插件直到只剩下 BentoBox.

如果该问题不再出现, 意味着这个问题由其他插件造成.
那样的话, 请参阅[此部分](#_10).

如果该问题依然出现, 意味着这可能是 BentoBox 的 bug.
请[在 GitHub 上汇报](https://github.com/BentoBoxWorld/BentoBox/issues).

#### 之后该如何清除已生成的超平坦区块?

如果你有备份, 请执行回档操作.

如果你没有备份, 登录你的服务器并使用 `/[admin-command] settings` 指令打开管理员控制面板.
查找 "*Clean Super Flat*" 选项并启用它.
根据你的个性化配置, 语言以及你所使用的 BentoBox 版本, 名称, 图标和描述或许并不一样.
但你一定会轻松找到该选项!

![image](https://m.360buyimg.com/babel/jfs/t1/87994/2/33484/321086/63e22029Fac4464c3/739eabd6135fbf92.png)  
*BSkyBlock 的清除超平坦区块选项*.

此功能将会**逐渐重新生成超平坦区块**.
这只会在区块加载时执行, 所以你可以传送到那些区块来强制重生成, 或是把这个选项开个几天.
**但请别忘了关闭此选项!**
它十分占用服务器资源...

### 服务器会在玩家创建岛屿时卡顿!

粘贴岛屿和区块生成是此问题的根源.

首先，粘贴速度可能设置的过高.
尝试减小它.
在 BentoBox 的 `config.yml` 查找此选项:

```yaml
# 粘贴蓝图时每 tick 粘贴的方块数量.
# 此值越小造成的卡顿越不明显但会导致岛屿粘贴时间变长.
# 相反, 此值越大粘贴所用时间越短, 但这可能会导致大量的区块加载
# 来使之完成, 而这通常会导致服务器崩溃.
paste-speed: 64
```

如果你正在使用 timings, `BlueprintPaster` 应该会运行较久.

如果服务器粘贴岛屿时仍然很卡顿, 那么可能是因为服务器正在生成区块.
这作为一个插件是很难控制的, 但这有一些方法来减轻此情况:

* 尝试在游戏模式扩展配置文件中减小 "distance between islands" 选项的值.
越小的值代表越少的区块生成.
这将需要你彻底重置岛屿世界和数据库.
* 使用 Paper 作为你的服务端.
Paper 可以异步生成区块.
* 预生成世界.
尤其是占用资源较多的游戏模式, 比如 CaveBlock 和 SkyGrid.

### 岛屿内无法放置树苗!

*相关问题:*
[BentoBox#277](https://github.com/BentoBoxWorld/BentoBox/issues/277).

如果没有任何消息提示该玩家不能放置树苗, 那么这就**不是** BentoBox 造成的问题.

如果你正在使用 **GriefPrevention** 插件, 在那个插件中有一个[选项](https://github.com/TechFortress/GriefPrevention/wiki/Setup-and-Configuration#preventing-tree-grief)会阻止玩家放置“位于空中的树”.

### 如何修改岛屿间距?

所有的游戏模式配置文件中都有岛屿间距设置. 在 BSkyBlock 中, 这个选项是 `distance-between-islands` 并位于 config.yml 的这个位置:

```
# 岛屿半径. (岛屿间距是这个的二倍)
  # 此选项将对所有维度生效 : 主世界, 下界以及末地.
  # 此选项不可中途更改。如果更改，插件将不会启动.
  # /!\ BentoBox 当前不支持中途更改此选项. 如果你确实需要更改, 请彻底重置你的世界和数据库.
  distance-between-islands: 400
```

拿 BSkyBlock 来说, 此项的默认值为 400, 即两个岛屿出生点之间的距离为 800. 同时这也意味着玩家的岛屿保护半径最大为 400.

大多数情况下, 默认值已经适合大多数服务器. 但是, 一些服主可能希望玩家的可支配范围更大或是更小. 但不管怎样, 一旦服务器已经开始运行, **此值就不可改变**. 如果你改变了它, BentoBox 将不会启动并给出这样的警告信息:

```
[14:08:20 ERROR]: [BentoBox] *****************CRITIAL ERROR!******************
[14:08:20 ERROR]: [BentoBox] Island distance mismatch!
World 'bskyblock_world' distance 800 != island range 400!
Island ID in database is BSkyBlock99ea1c15-f5f8-410a-9019-d6b843a5a254.
Island distance in config.yml cannot be changed mid-game! Fix config.yml or clean database.
[14:08:20 ERROR]: [BentoBox] Could not load islands! Disabling BentoBox...
[14:08:20 ERROR]: [BentoBox] *************************************************

[14:08:20 ERROR]: [BentoBox] *****************严重错误!******************
[14:08:20 ERROR]: [BentoBox] 岛屿间距改变!
世界 'bskyblock_world' 的岛屿间距为 800 与此前设置的 400 不相同!
数据库中的岛屿 ID 为 BSkyBlock99ea1c15-f5f8-410a-9019-d6b843a5a254.
config.yml 中的岛屿间距不可中途改变! 请还原 config.yml 配置或重置数据库.
[14:08:20 ERROR]: [BentoBox] 加载岛屿失败! 禁用 BentoBox 中...
[14:08:20 ERROR]: [BentoBox] *************************************************
```
这是一个保护机制, 因为如果使用了更改后的值, 岛屿可能会重叠!

** 我只是刚刚开服! 我该如何更改此选项并清理数据库? **

如果你用的是默认的 JSON 存储方式 (flat file). 请参考以下步骤:

1. 关闭服务器
2. 修改 config.yml 中的岛屿间距值.
3. 如果你没有同时安装其它游戏模式, 或是想重置所有数据, 那么直接删除 `plugins/BentoBox/database` 和 `plugins/BentoBox/database_backup` 文件夹即可
4. 删除游戏模式生成的世界, 以 BSkyBlock 为例, 默认为这几个文件夹: `bskyblock_world`, `bskyblock_world_nether`, 以及 `bskyblock_world_the_end` 
5. 启动服务器.

如果你已经在服务器上运行了其它游戏模式, 那么会稍复杂一点:
1. 关闭服务器
2. 修改 config.yml 中的岛屿间距值.
3. 打开 `plugins/BentoBox/database/Island` 文件夹并删除所有以游戏模式名称开头的文件, 例如, `BSkyBlock99ea1c15-f5f8-410a-9019-d6b843a5a254.json`
4. 删除游戏模式创建的世界, 以 BSkyBlock 为例, 默认应该为这几个文件夹: `bskyblock_world`, `bskyblock_world_nether`, 以及 `bskyblock_world_the_end` 
5. 启动服务器.

如果你使用的是 MySQL 等存储方式, 那么步骤是一样的, 但你需要使用 SQL 指令来清除数据库, 数据表, 或是内容.


### 如何启用下界传送门连接机制?

在 BentoBox 1.16 之后的版本中我们提供了一个选项来使下界门连接生效. 但这个选项只有 server.properties 中的 `allow-nether` 和 bukkit.yml 中的 `allow-end` 启用的时候才生效. 

要启用该功能, 请在配置文件中查找 `create-and-link-portals` 并将其设置为 `true`.

如果要启用黑曜石平台的生成 (就像原版末地那样) 请查找 `create-obsidian-platform` 并将其设为 `true`.

注意, 启用此选项将会和原版 Minecraft 一样出现无限的黑曜石.  


## API

### 如何制作一个 BentoBox 扩展? 有 API 吗?

有 API.
制作扩展与制作插件基本无异, 除了多了一些像团队、保护、指令、控制面板和蓝图粘贴之类的 API.

参考[此教程](Tutorials/api/Create-an-addon.md)来制作你的第一个扩展!
