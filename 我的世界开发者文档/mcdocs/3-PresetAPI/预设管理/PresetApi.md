# PresetApi

## 索引

接口 |  | 描述  
---|---|---  
[CreateTransform](<#createtransform>) | **[客户端]**/**[服务端]** | 构造变换对象  
[GetAllPresets](<#getallpresets>) | **[客户端]**/**[服务端]** | 获取所有预设  
[GetBlockPresetByPosition](<#getblockpresetbyposition>) | **[客户端]**/**[服务端]** | 获取指定位置的第一个方块预设  
[GetGameObjectByEntityId](<#getgameobjectbyentityid>) | **[客户端]**/**[服务端]** | 获取指定实体ID的游戏对象  
[GetGameObjectById](<#getgameobjectbyid>) | **[客户端]**/**[服务端]** | 获取指定对象ID的游戏对象  
[GetGameObjectByTypeName](<#getgameobjectbytypename>) | **[客户端]**/**[服务端]** | 获取指定类型和名称的第一个游戏对象  
[GetGameObjectsByTypeName](<#getgameobjectsbytypename>) | **[客户端]**/**[服务端]** | 获取指定类型和名称的所有游戏对象  
[GetPartApi](<#getpartapi>) | **[客户端]**/**[服务端]** | 获取零件API  
[GetPresetByName](<#getpresetbyname>) | **[客户端]**/**[服务端]** | 获取指定名称的第一个预设  
[GetPresetByType](<#getpresetbytype>) | **[客户端]**/**[服务端]** | 获取指定维度的指定类型的第一个预设  
[GetPresetSize](<#getpresetsize>) | **[客户端]**/**[服务端]** | 根据预设ID获取预设的包围盒大小  
[GetPresetsByName](<#getpresetsbyname>) | **[客户端]**/**[服务端]** | 获取指定名称的所有预设  
[GetPresetsByType](<#getpresetsbytype>) | **[客户端]**/**[服务端]** | 获取指定维度的指定类型的所有预设  
[GetTickCount](<#gettickcount>) | **[客户端]**/**[服务端]** | 获取当前帧数  
[LoadPartByModulePath](<#loadpartbymodulepath>) | **[客户端]**/**[服务端]** | 通过模块相对路径加载零件并实例化  
[LoadPartByType](<#loadpartbytype>) | **[客户端]**/**[服务端]** | 通过类名加载零件并实例化  
[SpawnPreset](<#spawnpreset>) | **[服务端]** | 在指定维度的指定坐标变换处生成指定预设  
  
## CreateTransform

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

构造变换对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
pos | tuple(float,float,float) | 位置变换  
rotation | tuple(float,float,float) | 旋转变换  
scale | tuple(float,float,float) | 缩放变换  
  * 返回值

数据类型 | 说明  
---|---  
Transform | 生成的变换对象  
  * 示例
```python
    # 创建Transform对象
    import Preset.Controller.PresetApi as presetApi
    transform = presetApi.CreateTransform((0, 0, 0), (0, 0, 0), (1, 1, 1))
```
## GetAllPresets

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取所有预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
list(PresetBase) | 预设列表  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    presets = presetApi.GetAllPresets()
```
## GetBlockPresetByPosition

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定位置的第一个方块预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | X轴坐标  
y | int | Y轴坐标  
z | int | Z轴坐标  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
BlockPreset | 指定位置的第一个方块预设，没有返回None  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetBlockPresetByPosition(0, 0, 0)
```
## GetGameObjectByEntityId

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定实体ID的游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
entityId | str | 指定的实体ID  
  * 返回值

数据类型 | 说明  
---|---  
TransformObject | 成功返回游戏对象，失败返回None  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetGameObjectByEntityId(0)
```
## GetGameObjectById

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定对象ID的游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
id | int | 指定的对象ID  
  * 返回值

数据类型 | 说明  
---|---  
TransformObject | 成功返回游戏对象，失败返回None  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetGameObjectById(0)
```
## GetGameObjectByTypeName

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定类型和名称的第一个游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 指定类型 (包括预设，零件，素材数据的类型)  
name | str | 指定名称，可缺省  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
TransformObject | 成功返回游戏对象，失败返回None  
  * 示例
```python
    # 找到第一个实体预设
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetGameObjectByTypeName("EntityPreset")
```
## GetGameObjectsByTypeName

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定类型和名称的所有游戏对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 指定类型 (包括预设，零件，素材数据的类型)  
name | str | 指定名称，可缺省  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
list(TransformObject) | 变换对象列表  
  * 示例
```python
    # 找到第一个实体预设
    import Preset.Controller.PresetApi as presetApi
    objects = presetApi.GetGameObjectsByTypeName("EntityPreset")
```
## GetPartApi

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取零件API

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
PartBase | 空零件，用于调用零件API  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    partApi = presetApi.GetPartApi()
    partApi.LogDebug("debug")
```
## GetPresetByName

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定名称的第一个预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
name | str | 指定名称  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
PresetBase | 预设/None  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetPresetByName("name")
```
## GetPresetByType

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定维度的指定类型的第一个预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 指定类型  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
PresetBase | 预设/None  
  * 示例
```python
    # 获取第一个实体预设
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetPresetByType("EntityPreset")
```
## GetPresetSize

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

根据预设ID获取预设的包围盒大小

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
presetId | str | 预设ID  
  * 返回值

数据类型 | 说明  
---|---  
tuple(float, | float, float) 预设的包围盒大小  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    size = presetApi.GetPresetSize(presetId)
```
## GetPresetsByName

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定名称的所有预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
name | str | 指定名称  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
list(PresetBase) | 预设列表  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetPresetsByName("name")
```
## GetPresetsByType

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取指定维度的指定类型的所有预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
classType | str | 指定类型  
dimension | int | 指定维度ID，默认为-1表示所有维度，本参数在客户端调用时无效，客户端只能获取当前维度  
  * 返回值

数据类型 | 说明  
---|---  
list(PresetBase) | 预设列表  
  * 示例
```python
    # 获取所有的空预设
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.GetPresetsByType("EmptyPreset")
```
## GetTickCount

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

获取当前帧数

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 当前帧数  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    cnt = presetApi.GetTickCount()
```
## LoadPartByModulePath

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

通过模块相对路径加载零件并实例化

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
modulePath | str | 零件模块相对路径  
  * 返回值

数据类型 | 说明  
---|---  
PartBase | 实例化后的零件，失败返回None  
  * 示例
```python
    # 加载内置的世界属性零件
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.LoadPartByModulePath("Preset.Parts.WorldPart")
    # 加载自定义零件，需要把Script_xxxxxx，YourPartDir，YourPart替换为你的自定义零件
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.LoadPartByModulePath("Script_xxxxxx.Parts.YourPartDir.YourPart")
```
## LoadPartByType

**[客户端]**/**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

通过类名加载零件并实例化

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
partType | str | 零件类名  
  * 返回值

数据类型 | 说明  
---|---  
PartBase | 实例化后的零件，失败返回None  
  * 示例
```python
    import Preset.Controller.PresetApi as presetApi
    obj = presetApi.LoadPartByType("WorldPart")
```
## SpawnPreset

**[服务端]**

method in Preset.Controller.PresetApi

  * 描述

在指定维度的指定坐标变换处生成指定预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
presetId | str | 指定预设的文件ID，对应预设对象的属性presetId  
transform | Transform | 指定的坐标变换(预设对象->通用->坐标变换Transform)  
dimension | int | 指定的维度ID  
virtual | bool | 素材是否已实例化到地图，True表示未实例化，会在首次加载该预设时实例化素材到地图存档，False则会忽略所有素材  
  * 返回值

数据类型 | 说明  
---|---  
PresetBase | 返回生成的预设，失败返回None  
  * 示例
```python
    # 内置空预设，特效预设的文件ID分别为EmptyPreset，EffectPreset
    import Preset.Controller.PresetApi as presetApi
    from Preset.Model.Transform import Transform
    preset = presetApi.SpawnPreset("EmptyPreset", Transform(), 0)
```