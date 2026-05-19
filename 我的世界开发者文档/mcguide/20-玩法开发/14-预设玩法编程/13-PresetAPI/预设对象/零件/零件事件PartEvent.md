# 零件事件PartEvent

## 索引

接口 |  | 描述  
---|---|---  
[OnTriggerEntityEnter](<#ontriggerentityenter>) | **[客户端]**/**[服务端]** | 触发器范围有实体进入时触发，只适用于TriggerPart  
[OnTriggerEntityExit](<#ontriggerentityexit>) | **[客户端]**/**[服务端]** | 触发器范围有实体离开时触发，只适用于TriggerPart  
[OnTriggerEntityStay](<#ontriggerentitystay>) | **[客户端]**/**[服务端]** | 触发器范围有实体停留时触发，只适用于TriggerPart  
  
## OnTriggerEntityEnter

**[客户端]**/**[服务端]**

method in Preset.Parts.PartEvent

  * 描述

触发器范围有实体进入时触发，只适用于TriggerPart

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
TriggerPart | PartBase | 发射事件的触发器零件  
EnterEntityIds | list(str) | 进入触发器范围的实体ID列表  
  * 返回值

无

  * 示例
```python
    part = self.GetParent().GetPartByType("TriggerPart")
    if not part:
        return
    self.ListenPartClientEvent(part.id, "OnTriggerEntityEnter", self, self.OnTriggerEntityEnter)
```
## OnTriggerEntityExit

**[客户端]**/**[服务端]**

method in Preset.Parts.PartEvent

  * 描述

触发器范围有实体离开时触发，只适用于TriggerPart

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
TriggerPart | PartBase | 发射事件的触发器零件  
ExitEntityIds | list(str) | 离开触发器范围的实体ID列表  
  * 返回值

无

  * 示例
```python
    part = self.GetParent().GetPartByType("TriggerPart")
    if not part:
        return
    self.ListenPartClientEvent(part.id, "OnTriggerEntityExit", self, self.OnTriggerEntityExit)
```
## OnTriggerEntityStay

**[客户端]**/**[服务端]**

method in Preset.Parts.PartEvent

  * 描述

触发器范围有实体停留时触发，只适用于TriggerPart

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
TriggerPart | PartBase | 发射事件的触发器零件  
StayEntityIds | list(str) | 停留在触发器范围的实体ID列表  
  * 返回值

无

  * 示例
```python
    part = self.GetParent().GetPartByType("TriggerPart")
    if not part:
        return
    self.ListenPartClientEvent(part.id, "OnTriggerEntityStay", self, self.OnTriggerEntityStay)
```