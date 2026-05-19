# 配置与Json文件关系

## 配套文件

使用编辑器的配置功能和直接修改Json文件制作Addon的本质是一样的，每个不同类型的配置都对应着不同的配套文件，修改配置，对应的Json文件也会实时改变。

这里我们以实体配置举例，当我们新建好实体配置后，选中即可在属性面板的配套文件处看到其对应的Json文件路径，点击右侧的打开文件（下图红框内）按钮，即可打开Json

![image](/dev/mcmanual/mc-dev/assets/img/open_json_file.8edf7922.png)

Json支持多种软件打开和查看，这里我们使用vscode进行查看。

![image](/dev/mcmanual/mc-dev/assets/img/openjsonfile2.8bafdc30.gif)

此时我们在配置界面增加字段，文件也会实时更新，如下图所示

![image](/dev/mcmanual/mc-dev/assets/img/editjsonfile.3d9dca64.gif)

  


关于如何进行配置的创建和使用可参考[配置的使用](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/0-配置.html>)，下面为你列出了当前编辑器支持的所有配置及其对应的文件:

> **点击标题的链接可以从Json文件层面深入了解自定义游戏内容的原理和作用：**

  


## [自定义实体](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/3-自定义生物/01-自定义基础生物.html>)配置对应文件

> 行为包json：behavior_pack_xxxxxx/entities/实体名称.json
> 
> 资源包json：resource_pack_xxxxxx/entity/实体名称.json
> 
> 语言文件(单个模组唯一)：resource_pack_xxxxxx/texts/zh_CN.lang

  


## [自定义物品](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/1-自定义物品/1-自定义基础物品.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_items_beh/物品名称.json**_
> 
> 资源包json： _**resource_pack_xxxxxx/netease_items_res/物品名称.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_
> 
> 盔甲穿戴属性文件： _**resource_pack_xxxxxx/textures/物品名称.json**_
> 
> 物品贴图文件(单个模组唯一)： _**resource_pack_xxxxxx/textures/item_texture.json**_

  


## [自定义方块](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/2-自定义方块/0-自定义方块概述.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_blocks/方块名称.json**_
> 
> 方块贴图文件(单个模组唯一):_**resource_pack_xxxxxx/textures/item_texture.json**_
> 
> 方块列表文件： _**resource_pack_xxxxxx/block.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_

  


## [自定义配方](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义配方.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_recipes/配方名称.json**_

  


## [ 自定义交易表 ](<../../../mconline/20-玩法地图教程/第05章：设置NPC的基本状态和交易表/课程03.给NPC添加对应的交易表.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/trading/交易表名称.json**_

  


## [ 自定义掉落表 ](<../../../mconline/10-addon教程/第12章：更完善的自定义掉落物/课程01.更完善的自定义掉落物.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/loot_tables/掉落表名称.json**_

  


## [自定义生成规则](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/4-自定义维度/3-生物生成.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/spawn_rules/生成规则名称.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_

  


## [自定义维度](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/4-自定义维度/1-自定义维度.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_dimension/维度配置名称.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_

  


## [自定义生物群系](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/4-自定义维度/2-群系地貌.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_biomes/生物群系配置名称.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_

  


## [自定义特征](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/4-自定义维度/4-自定义特征.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_features/特征名称.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_

  


## [自定义特征生成](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/4-自定义维度/4-自定义特征.html>)配置对应文件

> 行为包json： _**behavior_pack_xxxxxx/netease_feature_rules/特征生成规则.json**_
> 
> 语言文件(单个模组唯一)： _**resource_pack_xxxxxx/texts/zh_CN.lang**_

入门

分钟