# 预设基类PresetBase

## 概述

  * 继承关系


  * 描述

PresetBase（预设基类）是所有预设的基类。预设是一类可以被直接放置在场景中的TransformObject（变换对象），并且预设下可以挂接其他TransformObject，可以通过这种方式对游戏逻辑进行简单的封装。在编辑器中放置预设时，会生成预设的虚拟实例，在游戏中生成预设，会生成实例。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
presetId | str | 预设文件ID  
preLoad | bool | 是否预加载  
forceLoad | bool | 是否常加载  
childPresetInstances | list(PresetBase) | 子预设列表  
childPartInstances | list(PartBase) | 子零件列表  
dimension | int | 预设所在维度  
isAlive | bool | 预设是否存活  


## 索引

接口 |  | 描述  
---|---|---  
[GetIsAlive](<#getisalive>) | **[客户端]**/**[服务端]** | 获取预设的存活状态  
[GetGameObjectById](<#getgameobjectbyid>) | **[客户端]**/**[服务端]** | 获取当前预设节点底下指定ID的游戏对象  
[GetGameObjectByEntityId](<#getgameobjectbyentityid>) | **[客户端]**/**[服务端]** | 获取当前预设节点底下指定实体ID的游戏对象  
[GetChildPresets](<#getchildpresets>) | **[客户端]**/**[服务端]** | 获取当前预设的所有子预设  
[GetChildPresetsByName](<#getchildpresetsbyname>) | **[客户端]**/**[服务端]** | 获取指定名称的所有子预设  
[GetChildPresetsByType](<#getchildpresetsbytype>) | **[客户端]**/**[服务端]** | 获取指定类型的所有子预设  
[GetChildObjectByTypeName](<#getchildobjectbytypename>) | **[客户端]**/**[服务端]** | 获取指定实体ID的游戏对象  
[GetChildObjectsByTypeName](<#getchildobjectsbytypename>) | **[客户端]**/**[服务端]** | 获取指定实体ID的游戏对象  
[SetBlockProtect](<#setblockprotect>) | **[服务端]** | 设置预设内的所有素材区域的方块保护状态  
[Replicate](<#replicate>) | **[客户端]**/**[服务端]** | 在指定位置坐标下复制当前预设  
[RemoveChild](<#removechild>) | **[客户端]**/**[服务端]** | 移除指定的子节点对象  
[AddBoxData](<#addboxdata>) | **[客户端]**/**[服务端]** | 添加指定的素材数据  
[RemoveBoxData](<#removeboxdata>) | **[客户端]**/**[服务端]** | 移除指定的素材数据  
[AddPreset](<#addpreset>) | **[客户端]**/**[服务端]** | 添加指定预设作为子预设  
[RemovePreset](<#removepreset>) | **[客户端]**/**[服务端]** | 移除指定的子预设  
[AddPart](<#addpart>) | **[客户端]**/**[服务端]** | 添加指定零件作为子零件  
[RemovePart](<#removepart>) | **[客户端]**/**[服务端]** | 移除指定的子零件  
[GetPartsByName](<#getpartsbyname>) | **[客户端]**/**[服务端]** | 获取指定名称的所有子零件  
[GetPartByName](<#getpartbyname>) | **[客户端]**/**[服务端]** | 获取指定名称的第一个子零件  
[GetPartsByType](<#getpartsbytype>) | **[客户端]**/**[服务端]** | 获取指定类型的所有子零件  
[GetPartByType](<#getpartbytype>) | **[客户端]**/**[服务端]** | 获取指定类型的第一个子零件  
[RemovePartsByType](<#removepartsbytype>) | **[客户端]**/**[服务端]** | 移除指定类型的所有子零件  
  
## GetIsAlive

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取预设的存活状态

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | 是否存活  


## GetGameObjectById

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取当前预设节点底下指定ID的游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
id | int | 对象ID  
  * 返回值

数据类型 | 说明  
---|---  
GameObject | 成功返回游戏对象，失败返回None  


## GetGameObjectByEntityId

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取当前预设节点底下指定实体ID的游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
entityId | str | 实体ID  
  * 返回值

数据类型 | 说明  
---|---  
GameObject | 成功返回游戏对象，失败返回None  


## GetChildPresets

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取当前预设的所有子预设

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
list(PresetBase) | 子预设列表  


## GetChildPresetsByName

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定名称的所有子预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
name | str | 名称  
recursive | bool | 是否递归查找，默认为是  
  * 返回值

数据类型 | 说明  
---|---  
list(PresetBase) | 子预设列表  


## GetChildPresetsByType

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定类型的所有子预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 类型  
recursive | bool | 是否递归查找，默认为是  
  * 返回值

数据类型 | 说明  
---|---  
list(PresetBase) | 子预设列表  


## GetChildObjectByTypeName

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定实体ID的游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 指定类型  
name | str | 指定名称，可缺省  
  * 返回值

数据类型 | 说明  
---|---  
TransformObject | 成功返回游戏对象，失败返回None  
  * 示例
```python
    self.GetChildObjectByTypeName("PresetDebugPart")
```
## GetChildObjectsByTypeName

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定实体ID的游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 指定类型  
name | str | 指定名称，可缺省  
  * 返回值

数据类型 | 说明  
---|---  
TransformObject | 成功返回游戏对象，失败返回None  
  * 示例
```python
    self.GetChildObjectsByTypeName("PresetDebugPart")
```
## SetBlockProtect

**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

设置预设内的所有素材区域的方块保护状态

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
protect | bool | 保护/取消保护  
  * 返回值

无


## Replicate

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

在指定位置坐标下复制当前预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
pos | tuple(float,float,float) | 位置坐标  
  * 返回值

数据类型 | 说明  
---|---  
PresetBase | 返回复制的预设，失败返回None  


## RemoveChild

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

移除指定的子节点对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
child | TransformObject | 待移除的子对象  
  * 返回值

无


## AddBoxData

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

添加指定的素材数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
boxData | BoxData | 待添加的素材数据  
  * 返回值

无


## RemoveBoxData

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

移除指定的素材数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
boxData | BoxData | 待移除的素材数据  
  * 返回值

无


## AddPreset

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

添加指定预设作为子预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
preset | PresetBase | 待添加的预设  
  * 返回值

无


## RemovePreset

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

移除指定的子预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
preset | PresetBase | 待移除的子预设  
  * 返回值

无


## AddPart

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

添加指定零件作为子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
part | PartBase | 待添加的零件  
  * 返回值

无


## RemovePart

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

移除指定的子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
part | PartBase | 待移除的零件  
  * 返回值

无


## GetPartsByName

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定名称的所有子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
name | str | 零件名称  
  * 返回值

数据类型 | 说明  
---|---  
list(PartBase) | 零件列表  


## GetPartByName

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定名称的第一个子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
name | str | 零件名称  
  * 返回值

数据类型 | 说明  
---|---  
PartBase | 零件/None  


## GetPartsByType

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定类型的所有子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
type | str | 零件类名  
  * 返回值

数据类型 | 说明  
---|---  
list(PartBase) | 零件列表  


## GetPartByType

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

获取指定类型的第一个子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
type | str | 零件类名  
  * 返回值

数据类型 | 说明  
---|---  
PartBase | 零件/None  


## RemovePartsByType

**[客户端]**/**[服务端]**

method in Preset.Model.PresetBase.PresetBase

  * 描述

移除指定类型的所有子零件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
type | str | 零件类名  
  * 返回值

无