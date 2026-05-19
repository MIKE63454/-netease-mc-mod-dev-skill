# 零件PartBase

## 概述

  * 继承关系


  * 描述

PartBase（零件基类）是可以与零件进行绑定，而零件可以挂接在预设下，以实现带逻辑的预设的组装。所有的自定义零件都需要继承PartBase，预设系统下的大部分代码都需要写在自定义零件中。注意，自定义零件只有挂接到预设，并且在游戏中实例化才能生效。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
tickEnable | str | 是否启用零件tick  
replicated | list | 启用网络复制的字段列表  


## 索引

接口 |  | 描述  
---|---|---  
[InitClient](<#initclient>) | **[客户端]** | 客户端的零件对象初始化入口  
[InitServer](<#initserver>) | **[服务端]** | 服务端的零件对象初始化入口  
[TickClient](<#tickclient>) | **[客户端]** | 客户端的零件对象逻辑驱动入口  
[TickServer](<#tickserver>) | **[服务端]** | 服务端的零件对象逻辑驱动入口  
[UnloadClient](<#unloadclient>) | **[客户端]** | 客户端的零件对象卸载逻辑入口  
[UnloadServer](<#unloadserver>) | **[服务端]** | 服务端的零件对象卸载逻辑入口  
[DestroyClient](<#destroyclient>) | **[客户端]** | 客户端的零件对象销毁逻辑入口  
[DestroyServer](<#destroyserver>) | **[服务端]** | 服务端的零件对象销毁逻辑入口  
[CanAdd](<#canadd>) | **[客户端]**/**[服务端]** | 判断零件是否可以挂接到指定的父节点上  
[GetTickCount](<#gettickcount>) | **[客户端]**/**[服务端]** | 获取当前帧数  
[LogDebug](<#logdebug>) | **[客户端]**/**[服务端]** | 打印msg % args调试日志，仅PC开发包有效  
[LogInfo](<#loginfo>) | **[客户端]**/**[服务端]** | 打印msg % args消息日志  
[LogError](<#logerror>) | **[客户端]**/**[服务端]** | 打印msg % args错误日志  
[GetGameObjectById](<#getgameobjectbyid>) | **[客户端]**/**[服务端]** | 获取指定对象ID的游戏对象  
[GetGameObjectByEntityId](<#getgameobjectbyentityid>) | **[客户端]**/**[服务端]** | 获取指定实体ID的游戏对象  
[GetLoadedPlayers](<#getloadedplayers>) | **[客户端]**/**[服务端]** | 获取服务器所有玩家的ID列表  
[GetPlayerObject](<#getplayerobject>) | **[客户端]**/**[服务端]** | 获取玩家对象  
[GetEntityObject](<#getentityobject>) | **[客户端]**/**[服务端]** | 获取实体对象  
[GetEffectObject](<#geteffectobject>) | **[客户端]** | 获取特效对象  
[CreateEffectPreset](<#createeffectpreset>) | **[客户端]** | 创建特效对象  
[CreateTextboardPreset](<#createtextboardpreset>) | **[客户端]** | 创建文字面板预设对象  
[ListenForEvent](<#listenforevent>) | **[客户端]**/**[服务端]** | 监听指定的事件  
[UnListenForEvent](<#unlistenforevent>) | **[客户端]**/**[服务端]** | 反监听指定的事件  
[ListenForEngineEvent](<#listenforengineevent>) | **[客户端]**/**[服务端]** | 监听指定的引擎事件  
[UnListenForEngineEvent](<#unlistenforengineevent>) | **[客户端]**/**[服务端]** | 反监听指定的引擎事件  
[CreateEventData](<#createeventdata>) | **[客户端]**/**[服务端]** | 创建自定义事件的数据，eventData用于发送事件。创建的eventData可以理解为一个dict，可以嵌套赋值dict,list和基本数据类型，但不支持tuple  
[BroadcastEvent](<#broadcastevent>) | **[客户端]**/**[服务端]** | 广播事件，双端通用  
[BroadcastPresetSystemEvent](<#broadcastpresetsystemevent>) | **[客户端]**/**[服务端]** | 广播给预设系统  
[NotifyToServer](<#notifytoserver>) | **[客户端]** | 通知服务端触发事件  
[NotifyToClient](<#notifytoclient>) | **[服务端]** | 通知指定客户端触发事件  
[BroadcastToAllClient](<#broadcasttoallclient>) | **[服务端]** | 通知指所有客户端触发事件  
[ListenSelfEvent](<#listenselfevent>) | **[客户端]**/**[服务端]** | 监听来自自己的事件  
[UnListenSelfEvent](<#unlistenselfevent>) | **[客户端]**/**[服务端]** | 反监听来自自己的事件  
[ListenPartEvent](<#listenpartevent>) | **[客户端]**/**[服务端]** | 监听来自指定零件的事件  
[UnListenPartEvent](<#unlistenpartevent>) | **[客户端]**/**[服务端]** | 反监听来自指定零件的事件  
[ListenPresetSystemEvent](<#listenpresetsystemevent>) | **[客户端]**/**[服务端]** | 监听来自预设系统的事件  
[UnListenPresetSystemEvent](<#unlistenpresetsystemevent>) | **[客户端]**/**[服务端]** | 反监听来自预设系统的事件  
[DestroyStoryLines](<#destroystorylines>) | **[客户端]**/**[服务端]** | 手动销毁零件蓝图  
[GetSelf](<#getself>) | **[客户端]**/**[服务端]** | 获取零件自身  
[GetApi](<#getapi>) | **[客户端]**/**[服务端]** | 返回当前对象可使用的SDK API模块  
[IsPlayerSneaking](<#isplayersneaking>) | **[服务端]** | 是否潜行  
[GetPlayerHunger](<#getplayerhunger>) | **[服务端]** | 获取玩家饥饿度，展示在UI饥饿度进度条上，初始值为20，即每一个鸡腿代表2个饥饿度。 **饱和度(saturation)** ：玩家当前饱和度，初始值为5，最大值始终为玩家当前饥饿度(hunger)，该值直接影响玩家**饥饿度(hunger)** 。  
1）增加方法：吃食物。  
2）减少方法：每触发一次**消耗事件** ，该值减少1，如果该值不大于0，直接把玩家 **饥饿度(hunger)** 减少1。  
[SetPlayerHunger](<#setplayerhunger>) | **[服务端]** | 设置玩家饥饿度。  
  
## InitClient

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

客户端的零件对象初始化入口

  * 参数

无

  * 返回值

无


## InitServer

**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

服务端的零件对象初始化入口

  * 参数

无

  * 返回值

无


## TickClient

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

客户端的零件对象逻辑驱动入口

  * 参数

无

  * 返回值

无


## TickServer

**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

服务端的零件对象逻辑驱动入口

  * 参数

无

  * 返回值

无


## UnloadClient

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

客户端的零件对象卸载逻辑入口

  * 参数

无

  * 返回值

无


## UnloadServer

**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

服务端的零件对象卸载逻辑入口

  * 参数

无

  * 返回值

无


## DestroyClient

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

客户端的零件对象销毁逻辑入口

  * 参数

无

  * 返回值

无


## DestroyServer

**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

服务端的零件对象销毁逻辑入口

  * 参数

无

  * 返回值

无


## CanAdd

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

判断零件是否可以挂接到指定的父节点上

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
parent | PresetBase | 即将挂接的父预设  
  * 返回值

数据类型 | 说明  
---|---  
str | 不允许挂接时，返回对应的错误提示  


## GetTickCount

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

获取当前帧数

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 当前帧数  


## LogDebug

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

打印msg % args调试日志，仅PC开发包有效

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
msg | str | 要打印的格式化字符串  
*args | list(object) | 格式化参数列表  
  * 返回值

无

  * 示例
```python
    self.LogDebug(“self.isClient: %s”, self.isClient)
```
## LogInfo

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

打印msg % args消息日志

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
msg | str | 要打印的格式化字符串  
*args | list(object) | 格式化参数列表  
  * 返回值

无

  * 示例
```python
    self.LogInfo(“self.isClient: %s”, self.isClient)
```
## LogError

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

打印msg % args错误日志

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
msg | str | 要打印的格式化字符串  
*args | list(object) | 格式化参数列表  
  * 返回值

无

  * 示例
```python
    self.LogError(“self.isClient: %s”, self.isClient)
```
## GetGameObjectById

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

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
    obj = self.GetGameObjectById(0)
```
## GetGameObjectByEntityId

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

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
    obj = self.GetGameObjectByEntityId(0)
```
## GetLoadedPlayers

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

获取服务器所有玩家的ID列表

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
list(str) | 所有玩家的ID列表  


## GetPlayerObject

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

获取玩家对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
playerId | str | 玩家ID  
  * 返回值

数据类型 | 说明  
---|---  
PlayerObject | 成功返回玩家对象，失败返回None  
  * 示例
```python
    player = self.GetPlayerObject(playerId)
```
## GetEntityObject

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

获取实体对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
entityId | str | 指定的实体ID  
  * 返回值

数据类型 | 说明  
---|---  
EntityObject | 成功返回实体对象，失败返回None  
  * 示例
```python
    entity = self.GetEntityObject(entityId)
```
## GetEffectObject

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

获取特效对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
effectId | int | 特效ID  
  * 返回值

数据类型 | 说明  
---|---  
EffectObject | 成功返回特效对象，失败返回None  
  * 示例
```python
    effect = self.GetEffectObject(effectId)
```
## CreateEffectPreset

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

创建特效对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
resource | str | 特效资源json  
pos | tuple(float,float,float) | 特效位置  
rotation | tuple(float,float,float) | 特效旋转  
scale | tuple(float,float,float) | 特效缩放  
  * 返回值

数据类型 | 说明  
---|---  
EffectPreset | 成功返回特效对象，失败返回None  
  * 示例
```python
    # 在某个实体位置播放指定特效
    effect = self.CreateEffectPreset("effects/xxx", self.GetEntityPos(entityId))
```
## CreateTextboardPreset

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

创建文字面板预设对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
text | str | 文字显示内容  
textColor | tuple(float,float,float,float) | 文字颜色的RGBA值，范围0-1  
boardColor | tuple(float,float,float,float) | 可选参数，默认None，设置为黑色，面板颜色的RGBA值，范围0-1  
pos | tuple(float,float,float) | 可选参数，默认(0, 0, 0) 生成文字面板位置  
faceCamera | bool | 是否始终朝向相机, 默认为True  
  * 返回值

数据类型 | 说明  
---|---  
TextboardPreset | 成功返回文字面板对象，失败返回None  
  * 示例
```python
    # 在某个位置生成文字面板预设
    textboard = self.CreateTextboardPreset('Hello', (0.5, 0.4, 0.3, 0.8))
```
## ListenForEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

监听指定的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
namespace | str | 命名空间  
systemName | str | 事件系统名称  
eventName | str | 事件名称  
instance | object | 实例  
func | object | 函数  
priority | str | 优先级  
  * 返回值

无


## UnListenForEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

反监听指定的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
namespace | str | 命名空间  
systemName | str | 事件系统名称  
eventName | str | 事件名称  
instance | object | 实例  
func | object | 函数  
priority | str | 优先级  
  * 返回值

无


## ListenForEngineEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

监听指定的引擎事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
instance | object | 实例  
func | object | 函数  
priority | str | 优先级  
  * 返回值

无


## UnListenForEngineEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

反监听指定的引擎事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
instance | object | 实例  
func | object | 函数  
priority | str | 优先级  
  * 返回值

无


## CreateEventData

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

创建自定义事件的数据，eventData用于发送事件。创建的eventData可以理解为一个dict，可以嵌套赋值dict,list和基本数据类型，但不支持tuple

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
dict | 事件数据  


## BroadcastEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

广播事件，双端通用

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
eventData | object | 事件数据  
  * 返回值

无


## BroadcastPresetSystemEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

广播给预设系统

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
eventData | object | 事件数据  
  * 返回值

无


## NotifyToServer

**[客户端]**

method in Preset.Model.PartBase.PartBase

  * 描述

通知服务端触发事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
eventData | object | 事件数据  
  * 返回值

无


## NotifyToClient

**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

通知指定客户端触发事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
playerId | str | 玩家ID  
eventName | str | 事件名称  
eventData | object | 事件数据  
  * 返回值

无


## BroadcastToAllClient

**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

通知指所有客户端触发事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
eventData | object | 事件数据  
  * 返回值

无


## ListenSelfEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

监听来自自己的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
target | object | 目标  
func | object | 回调函数  
  * 返回值

无


## UnListenSelfEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

反监听来自自己的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
target | object | 目标  
func | object | 回调函数  
  * 返回值

无


## ListenPartEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

监听来自指定零件的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
partId | int | 指定零件的ID  
eventName | str | 事件名称  
target | object | 目标  
func | object | 回调函数  
  * 返回值

无


## UnListenPartEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

反监听来自指定零件的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
partId | int | 指定零件的ID  
eventName | str | 事件名称  
target | object | 目标  
func | object | 回调函数  
  * 返回值

无


## ListenPresetSystemEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

监听来自预设系统的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
target | object | 目标  
func | object | 回调函数  
  * 返回值

无


## UnListenPresetSystemEvent

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

反监听来自预设系统的事件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
eventName | str | 事件名称  
target | object | 目标  
func | object | 回调函数  
  * 返回值

无


## DestroyStoryLines

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

手动销毁零件蓝图

  * 参数

无

  * 返回值

无


## GetSelf

**[客户端]**/**[服务端]**

method in Preset.Model.PartBase.PartBase

  * 描述

获取零件自身

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
PartBase | 零件自身  


## GetApi

**[客户端]**/**[服务端]**

method in Preset.Model.SdkInterface.SdkInterface

  * 描述

返回当前对象可使用的SDK API模块

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
extraClientApi或extraServerApi | 返回当前对象可使用的SDK API模块  


## IsPlayerSneaking

**[服务端]**

method in Preset.Model.SdkInterface.SdkInterface

  * 描述

是否潜行

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
playerId | str或int | 实体id  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否潜行  
  * 示例
```python
    self.IsPlayerSneaking(playerId)
```
## GetPlayerHunger

**[服务端]**

method in Preset.Model.SdkInterface.SdkInterface

  * 描述

获取玩家饥饿度，展示在UI饥饿度进度条上，初始值为20，即每一个鸡腿代表2个饥饿度。 **饱和度(saturation)** ：玩家当前饱和度，初始值为5，最大值始终为玩家当前饥饿度(hunger)，该值直接影响玩家**饥饿度(hunger)** 。  
1）增加方法：吃食物。  
2）减少方法：每触发一次**消耗事件** ，该值减少1，如果该值不大于0，直接把玩家 **饥饿度(hunger)** 减少1。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
playerId | str或int | 玩家id  
  * 返回值

数据类型 | 说明  
---|---  
float | 玩家饥饿度  
  * 示例
```python
    self.GetPlayerHunger(playerId)
```
## SetPlayerHunger

**[服务端]**

method in Preset.Model.SdkInterface.SdkInterface

  * 描述

设置玩家饥饿度。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
playerId | str或int | 玩家id  
value | float | 饥饿度  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetPlayerHunger(playerId, 10)
```