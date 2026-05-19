# 方块预设BlockPreset

## 概述

  * 继承关系


  * 描述

BlockPreset（方块预设）是一类绑定方块的预设。由于MC的方块数量巨大，将方块预设与MC的原生方块绑定，尤其是地图中常见的原生方块可能对性能造成重大影响。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
engineTypeStr | str | 方块类型ID  
blockId | str | 方块类型数字ID  
auxValue | int | 附加值  


## 索引

接口 |  | 描述  
---|---|---  
[GetEngineTypeStr](<#getenginetypestr>) | **[客户端]**/**[服务端]** | 获取方块预设的方块类型ID  
  
## GetEngineTypeStr

**[客户端]**/**[服务端]**

method in Preset.Model.Block.BlockPreset.BlockPreset

  * 描述

获取方块预设的方块类型ID

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | 方块类型ID