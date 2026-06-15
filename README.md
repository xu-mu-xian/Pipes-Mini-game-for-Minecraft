[Read in English](README_en.md)

# 接水管小游戏数据包

本数据包支持版本：26.1.2

官方命名空间: `pipes`

![](readme/logo.png)

## 1. 如果你是玩家

**请务必下载安装配套的资源包，否则游戏界面会变得很奇怪！**

**如何正常开始？** 你可以按以下的配方合成得到一个**接水管触发器**。实际上这个物品是一个**胡萝卜钓竿**，所以它会不可避免地吸引到猪，并能控制猪的骑乘。

![](readme/recipe.png)

手持接水管触发器按下 `鼠标右键`，即可打开接水管小游戏的主页面。此物品可使用无限次。

你可以依靠以下命令很快捷地获得这个物品：

```mcfunction
loot give @s loot pipes:pipes_trigger
```

在游玩的过程中，你可能会遇到游戏界面超出当前屏幕宽度、高度，或者是游戏网格没有居中的情况，这和你当前客户端的分辨率和界面尺寸有关。很遗憾，Mojang 并没有给我们提供任何读取分辨率和界面尺寸的手段，所以这些设置只能由你亲自调整以适应你的屏幕。

---

## 2. 如果你是开发者

你不仅可以直接把数据包安装进自己的世界游玩，也可以将此数据包内嵌进你自己的数据包，或是作为你大作品的前置数据包使用。注意，**你必须同意、遵守此数据包的相关许可。**

如果你将此数据包作为另一个数据包的子数据包使用，部分文件路径会产生冲突，在元数据的 `overlays` 指定对应的项之后，你可以依照以下步骤修改此数据包：

1. 删去 `minecraft` 命名空间下的所有内容。
1. 使 `pipes:load` 这个函数被主数据包的 `#minecraft:load` 标签或其调用的函数、子函数调用。

资源包的内置情况以此类推。

**注意！本数据包做出了以下可能会导致冲突的改动！**

```mcfunction
gamerule send_command_feedback false
gamerule max_command_sequence_length 1000000
```

以下是针对此数据包一些开发性功能的介绍：

### 2.1 使用命令直接生成接水管谜题

- 你可以使用以下方式指定网格的宽度和高度，并生成一道接水管谜题：
  ```mcfunction
  function pipes:generate/random {w:<宽度>,h:<高度>}
  ```

- 允许使用种子以生成固定的谜题：
  ```mcfunction
  function pipes:generate/seed {w:<宽度>,h:<高度>,seed:<种子>}
  ```

  上述传入的数据都应为整型。

- 上一次生成的谜题可用过以下命令恢复：
  ```mcfunction
  scoreboard players set <玩家> pipes.trigger 2
  ```

  其中 `<玩家>` 可以是任意需要触发此功能的玩家。此功能仅限恢复上一次的谜题，在此之前的谜题无法恢复。

- 快速生成以下大小的接水管（仅当设置 `trigger` 为 `true` 时有效，见小节 [2.2.3](#222-trigger)）：
  | 大小 | 命令 |
  | --- | --- |
  | $4 \times 4$ | `scoreboard players set <玩家> pipes.trigger 4` |
  | $5 \times 5$ | `scoreboard players set <玩家> pipes.trigger 5` |
  | $6 \times 6$ | `scoreboard players set <玩家> pipes.trigger 6` |
  | $7 \times 7$ | `scoreboard players set <玩家> pipes.trigger 7` |
  | $8 \times 8$ | `scoreboard players set <玩家> pipes.trigger 8` |
  | $9 \times 9$ | `scoreboard players set <玩家> pipes.trigger 9` |
  | $10 \times 10$ | `scoreboard players set <玩家> pipes.trigger 10` |
  | $11 \times 11$ | `scoreboard players set <玩家> pipes.trigger 11` |
  | $12 \times 12$ | `scoreboard players set <玩家> pipes.trigger 12` |
  | $13 \times 13$ | `scoreboard players set <玩家> pipes.trigger 13` |
  | $14 \times 14$ | `scoreboard players set <玩家> pipes.trigger 14` |
  | $15 \times 15$ | `scoreboard players set <玩家> pipes.trigger 15` |
  | $16 \times 16$ | `scoreboard players set <玩家> pipes.trigger 16` |

- 调出接水管小游戏主界面的方式：
  ```mcfunction
  scoreboard players set <玩家> pipes.trigger 1
  ```

### 2.2 游戏设置

该小游戏安排了一些设置选项，它们的数据被存在命令存储 `pipes:grid` 的字段 `settings` 中，此字段是一个复合标签，其中每一个子标签都是一个单独的设置选项。**你可以直接使用 `/data` 修改选项的数据，也可以按照下文的方式修改选项数据。其中后者更推荐，因为修改选项的同时也可能会有伴生操作。**

#### 2.2.1 record

布尔型选项，有效值 `true` 和 `false`。此选项用于控制每局游戏是否计时，玩家在接水管菜单页面可自由修改，默认为 `true`。

- 设为 `true` 所需的命令：`function pipes:settings/record/true`
- 设为 `false` 所需的命令：`function pipes:settings/record/false`

#### 2.2.2 rotation_direction

字符串类型选项，有效值 `clockwise`（顺时针）和 `counterclockwise`（逆时针），用于控制玩家旋转管道的方向，可在菜单页面自由修改，默认为 `clockwise`。

- 设为 `clockwise` 所需的命令：`function pipes:settings/rotation_direction/clockwise`
- 设为 `counterclockwise` 所需的命令：`function pipes:settings/rotation_direction/counterclockwise``

#### 2.2.3 trigger

布尔型选项，有效值 `true` 和 `false`。此选项用于控制是否启用接水管小游戏菜单，此选项不能在菜单页面控制，默认为 `true`。若此值为 `false`，则所有菜单的按钮都被禁用，接水管触发器呼出接水管主菜单的操作也被禁用。**如果你不希望玩家在你自己的作品中能随意打开并游玩接水管，而让接水管更像是各种场景的特制内容，那你需要将此选项的值设为 `false`。**

- 设为 `true` 所需的命令：`function pipes:settings/trigger/true`
- 设为 `false` 所需的命令：`function pipes:settings/trigger/false`

### 2.3 定制化接水管

除了接水管本身的程序之外，游戏界面上所有的内容都是可以自定义的，包括接水管的网格纹理。

![](readme/dialog.png)

当你大量修改这些内容，而后又希望恢复默认的内容时，以下命令可以供你一键恢复所有的默认内容：

```mcfunction
function pipes:custom/reset
```

或

```mcfunction
function #pipes:custom_reset
```

#### 2.3.1 标题

对话框标题显示的内容，默认显示“接水管 <宽> x <高> 种子：<此题的种子>”，在存储内它的默认值是 `{translate:"dialog.pipes.game.title"}`。

你可以任意修改这个标题，让它成为你需要的样子。**标题的数据存储于命令存储 `pipes:grid` 的路径 `dialog.game.title`，值需要是一个文本组件。** 例如，你不希望这个标题显示任何内容：

```mcfunction
data modify storage pipes:grid dialog.game.title set value ""
```

如果你想让标题显示纯文本“管道修理处A”，你可以执行以下命令：

```mcfunction
data modify storage pipes:grid dialog.game.title set value "管道修理处A"
```

如果你想恢复默认的标题内容，可以执行以下命令：

```mcfunction
function pipes:custom/title/reset
```

#### 2.3.2 网格上方的文本

即显示于接水管游戏网格上方的文本，默认情况下它是空字符，不会有任何内容。它的数据存储于命令存储 `pipes:grid` 的路径 `dialog.game.body.contents[1]`。为了满足某些可能存在的需求，网格上方的文本与网格之间并没有默认添加换行符。如果你不在文本末尾添加换行符：

```mcfunction
data modify storage pipes:grid dialog.game.body.contents[1] set value "Text above"
```

它会看起来像这样：

![](readme/text_above.png)

如果你不希望它显示成这样，你需要在文字后面加上换行符：

```mcfunction
data modify storage pipes:grid dialog.game.body.contents[1] set value "Text above\n\n"
```

如果你想恢复默认的内容，可以执行以下命令：

```mcfunction
function pipes:custom/text_above/reset
```

#### 2.3.3 网格

虽然不建议你修改网格渲染的逻辑，但是你可以修改网格的纹理，甚至让每一个接水管谜题都有不一样的外观。

网格的纹理用自定义字体来实现。每一个格子都是单独的一个码位，下表列举了所有形状的管道对应的码位：

| 管道类型（开放状态） | 码位对应的字符 | 管道类型（锁定状态） | 码位对应的字符 |
| --- | --- | --- | --- |
| ![](resourcepack/assets/pipes/textures/font/none/source/1.png) | a | ![](resourcepack/assets/pipes/textures/font/none/source/1_lock.png) | A |
| ![](resourcepack/assets/pipes/textures/font/none/source/2.png) | b | ![](resourcepack/assets/pipes/textures/font/none/source/2_lock.png) | B |
| ![](resourcepack/assets/pipes/textures/font/none/source/3.png) | c | ![](resourcepack/assets/pipes/textures/font/none/source/3_lock.png) | C |
| ![](resourcepack/assets/pipes/textures/font/none/source/4.png) | d | ![](resourcepack/assets/pipes/textures/font/none/source/4_lock.png) | D |
| ![](resourcepack/assets/pipes/textures/font/none/straight/1.png) | e | ![](resourcepack/assets/pipes/textures/font/none/straight/1_lock.png) | E |
| ![](resourcepack/assets/pipes/textures/font/none/straight/2.png) | f | ![](resourcepack/assets/pipes/textures/font/none/straight/2_lock.png) | F |
| ![](resourcepack/assets/pipes/textures/font/none/turn/12.png) | g | ![](resourcepack/assets/pipes/textures/font/none/turn/12_lock.png) | G |
| ![](resourcepack/assets/pipes/textures/font/none/turn/23.png) | h | ![](resourcepack/assets/pipes/textures/font/none/turn/23_lock.png) | H |
| ![](resourcepack/assets/pipes/textures/font/none/turn/34.png) | i | ![](resourcepack/assets/pipes/textures/font/none/turn/34_lock.png) | I |
| ![](resourcepack/assets/pipes/textures/font/none/turn/41.png) | j | ![](resourcepack/assets/pipes/textures/font/none/turn/41_lock.png) | J |
| ![](resourcepack/assets/pipes/textures/font/none/trible/1.png) | k | ![](resourcepack/assets/pipes/textures/font/none/trible/1_lock.png) | K |
| ![](resourcepack/assets/pipes/textures/font/none/trible/2.png) | l | ![](resourcepack/assets/pipes/textures/font/none/trible/2_lock.png) | L |
| ![](resourcepack/assets/pipes/textures/font/none/trible/3.png) | m | ![](resourcepack/assets/pipes/textures/font/none/trible/3_lock.png) | M |
| ![](resourcepack/assets/pipes/textures/font/none/trible/4.png) | n | ![](resourcepack/assets/pipes/textures/font/none/trible/4_lock.png) | N |

**一个管道类型有 4 个变种：普通（`normal`）、灌水（`flood`）、警告（`warning`）和水源（`source`）。命令存储 `pipes:grid` 中有这么一个字段 `style`，里面存储了各变种使用的字体的命名空间 ID。** 默认值如下：

```
style: {
  flood:"pipes:tube_flooded",
  normal:"pipes:tube",
  source:"pipes:tube_source",
  warning:"pipes:tube_warning"
}
```

你可以根据上表的码位制作自定义字体，然后修改存储中对应变种使用的字体，程序会自动应用你所制作的字体。例如，你给 `normal` 类的管道制定了一套自定义字体 `custom:your_style`，那么你可以按以下方式修改网格为你设计的样式：

```mcfunction
data modify storage pipes:grid style.normal set value "custom:your_style"
```

注意，这样只会修改 `normal` 类的管道外观，其他变种的管道外观不会因此而发生更改。

你可以执行以下命令以恢复默认的外观：

```mcfunction
function pipes:custom/style/reset
```

#### 2.3.4 已完成提示和时间记录

当玩家解开当前接水管题的时候，网格下方会出现一行字：“已完成！”，如果设置 `record` 为 `true`，那后面还会有此局游戏用时。同样的，你也可以自定义这行文本。当设置 `record` 为 `true` 时，这行文本被存储在命令存储 `pipes:grid` 的路径 `record.is_true`。它的默认值为

```
{translate:"dialog.pipes.game.time",with:[{color:"gold",text:""},{color:"gold",text:""},{color:"gold",text:""}]}
```

当设置 `record` 为 `false` 时，这行文本被存储在命令存储 `pipes:grid` 的路径 `record.is_false`。它的默认值为

```
{translate:"dialog.pipes.game.complete"}
```

如果你想修改设置 `record` 为 `true` 时显示的文本，比如修改成“Done!”，你可以执行以下命令：

```
data modify storage pipes:grid record.is_true set value "Done!"
```

你可以为这段文本传入本局游戏所耗费的时间，方法是使用本地化文本以及 `with`，其中：

- `with` 的第 0 个元素会自动传入分钟
- `with` 的第 1 个元素会自动传入秒的整数部分
- `with` 的第 2 个元素会自动传入秒的小数部分

例如，你希望的文本是“Done within xxx minutes and xxx seconds”，其中需要传入的参数是分钟和秒的整数部分，那你应该这么写：

```
data modify storage pipes:grid record.is_true set value {translate:"Done within %1$s minutes and %2$s seconds",with:[{text:""},{text:""}]}
```

参数传入的路径是 `text`，纯字符串形式不予识别，因此 `with` 列表内的元素都应为带 `text` 的复合标签形式。

如果你想无视设置 `record`，无论如何都显示同样的内容，比如修改成“Done!”，那你需要同时修改两个路径：

```mcfunction
data modify storage pipes:grid record.is_true set value "Done!"
data modify storage pipes:grid record.is_false set value "Done!"
```

恢复默认的内容可以执行以下命令：

```mcfunction
function pipes:custom/record/reset
```

#### 2.3.5 网格下方的文本

即显示于接水管游戏网格下方的文本，和网格上方的文本一样，默认情况下它是空字符，不会有任何内容。它的数据存储于命令存储 `pipes:grid` 的路径 `dialog.game.body.contents[1]`。同样，这段文本与已完成提示和时间记录之间并没有默认添加换行符，如果没有换行符，文本会直接显示在已完成提示和时间记录的后面。

用例：

```mcfunction
data modify storage pipes:grid dialog.game.body.contents[1] set value "\n\nText below"
```

如果你想恢复默认的内容，可以执行以下命令：

```mcfunction
function pipes:custom/text_below/reset
```

#### 2.3.6 操作按钮

即对话框内的功能性按钮，默认的 3 个按钮是“新游戏”、“锁定”和“重玩”，你也可以任意指定、删除这些按钮，但是必须保证至少存在 1 个任意的按钮。按钮的数据存储于命令存储 `pipes:grid` 的路径 `dialog.game.actions`，值必须为一个列表，列表内每个元素必须是一个按钮。它的默认值为

```
[{action:{command:"trigger pipes.trigger set 3",type:"minecraft:run_command"},label:{translate:"dialog.pipes.game.new_game"},width:80},{action:{command:"trigger pipes.trigger set 19",type:"minecraft:run_command"},label:{translate:"dialog.pipes.game.lock"},tooltip:{translate:"dialog.pipes.game.lock.tooltip"},width:80},{action:{command:"trigger pipes.trigger set 17",type:"minecraft:run_command"},label:{translate:"dialog.pipes.game.replay"},width:80}]
```

如果只保留“重玩”一个按钮的话，可以直接写成这样：

```mcfunction
data modify storage pipes:grid dialog.game.actions set value [{action:{command:"trigger pipes.trigger set 17",type:"minecraft:run_command"},label:{translate:"dialog.pipes.game.replay"},width:80}]
```

`trigger pipes.trigger set 17` 是内部的特定记分项操作，具体见小节 [2.4.1](#241-trigger-记分项)，但这个命令起作用的前提是设置 `record` 的值为 `true`。你可以使用自己的命令、自己的记分项。

恢复默认的按钮可以执行以下命令：

```mcfunction
function pipes:custom/buttons/reset
```

#### 2.3.7 退出按钮

游戏界面的对话框有一个 `exit_action`，如果你不想要这个按钮，可以把它去掉：

```mcfunction
function pipes:custom/exit_action/remove
```

或者任意自定义一个退出按钮：

```mcfunction
data modify storage pipes:grid dialog.game.exit_action set value <按钮>
```

恢复默认的退出按钮可以执行以下命令：

```mcfunction
function pipes:custom/exit_action/reset
```

### 2.4 详细程序

下面介绍本数据包程序性的一些功能。

#### 2.4.1 trigger 记分项

`pipes.trigger` 是触发器类型的记分项，用于对话框的点击事件。当然，**这些点击事件只有当设置 `record` 的值为 `true` 才有效**。各个分数映射的点击事件如下表所示：

| 分数 | 事件 |
| --- | --- |
| 1 | 打开小游戏主菜单 |
| 2 | 恢复上一局游戏 |
| 3 | （用上一局的网格宽高）开始新游戏 |
| 4 | 快速生成 $4 \times 4$ 大小的接水管题 |
| 5 | 快速生成 $5 \times 5$ 大小的接水管题 |
| 6 | 快速生成 $6 \times 6$ 大小的接水管题 |
| 7 | 快速生成 $7 \times 7$ 大小的接水管题 |
| 8 | 快速生成 $8 \times 8$ 大小的接水管题 |
| 9 | 快速生成 $9 \times 9$ 大小的接水管题 |
| 10 | 快速生成 $10 \times 10$ 大小的接水管题 |
| 11 | 快速生成 $11 \times 11$ 大小的接水管题 |
| 12 | 快速生成 $12 \times 12$ 大小的接水管题 |
| 13 | 快速生成 $13 \times 13$ 大小的接水管题 |
| 14 | 快速生成 $14 \times 14$ 大小的接水管题 |
| 15 | 快速生成 $15 \times 15$ 大小的接水管题 |
| 16 | 快速生成 $16 \times 16$ 大小的接水管题 |
| 17 | 重做当前的题 |
| 18 | 打开自定义游戏页面 |
| 19 | 在游戏过程中进入锁定模式 |
| 20 | 从锁定模式退出 |
| 21 | 打开设置页面 |
| 33 ~ 2727 | 自定义网格大小而后直接生成随机的题 |
| 99900 ~ | 调整设置 |
| -2727 ~ -33 | 自定义网格大小而后指定种子以生成固定的题 |

#### 2.4.2 题目生成步骤

函数 `pipes:generate/process` 用于生成一个接水管题目，你使用小节 [2.1](#21-使用命令直接生成接水管谜题) 所述的函数时，它们就会调用这个函数。当然，在生成之前需要确定网格的宽、高、种子（可选）。宽由 `#width` 在记分项 `pipes.var` 上持有，高由 `#height` 在记分项 `pipes.var` 上持有。种子是给随机序列 `pipes:prim` 使用的，方式为

```mcfunction
random reset pipes:prim <种子>
```

`pipes:generate/process` 内部依次会有以下步骤：

- 初始化：
  ```mcfunction
  function pipes:generate/init
  ```

- 生成地图：
  ```mcfunction
  function pipes:prim/
  ```

  这条命令执行完毕后，会产生一个完整的树，数据全部存入命令存储 `pipes:grid` 的路径 `grid`。这便是该接水管题目的原始树。此时数据还未可视化。

- 打乱管道：
  ```mcfunction
  function pipes:upset/
  ```

  此处的打乱是针对全局的随机打乱。如果你想自行对特定的格子随机旋转，可以执行以下命令：
  ```mcfunction
  function pipes:upset/manual {x:<格子的x坐标>,y:<格子的y坐标>}
  ```

  一条命令只能修改一个格子。`grid` 内的数据按行列对应实际网格内的节点，网格左上角的节点为 `grid[0][0]`，宽度方向的坐标 $x$ 向右增大，高度方向的坐标 $y$ 向下增大，右下角的节点为 `grid[<width-1>][<height-1>]`。

  你也可以针对一个格子让它旋转特定的次数，旋转方向为顺时针，命令为：
  ```mcfunction
  function pipes:upset/manual_with_rotate {x:<格子的x坐标>,y:<格子的y坐标>,rotate:<旋转次数>}
  ```

  注意，上述传入的参数必须都为整型，`rotate` 接受的值为介于 1~3（含）之间的整数。

- 解题判定：
  ```mcfunction
  function pipes:operation/tarjan/
  ```
  
- 显示生成结果：
  ```mcfunction
  function pipes:display/
  ```

你可以在自己的函数中任意组合这些流程以达到你想要的效果。例如，生成一个 $5 \times 5$ 大小的树，只随机旋转格子 `[3][3]`，然后渲染出来：

```mcfunction
scoreboard players set #height pipes.var 5
scoreboard players set #width pipes.var 5
function pipes:prim/
function pipes:upset/manual {x:3,y:3}
function pipes:display/
```