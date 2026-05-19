# 实体零件EntityBasePart

## 概述

  * 描述

实体零件

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
engineType | str | 实体类型  
autoCreate | bool | 是否在零件初始化时，自动创建关联实体，默认为True  
persistence | bool | 创建的实体是否持久化，默认为False，设为True时，创建完实体会将autoCreate重置为False  


## 索引

接口 |  | 描述  
---|---|---  
[CreateVirtualEntity](<#createvirtualentity>) | **[服务端]** | 手动创建关联实体，如果已创建会直接返回  
[DestroyVirtualEntity](<#destroyvirtualentity>) | **[服务端]** | 移除已创建的关联实体，引擎退出时会默认调用  
  
## CreateVirtualEntity

**[服务端]**

method in Preset.Parts.EntityBasePart.EntityBasePart

  * 描述

手动创建关联实体，如果已创建会直接返回

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | 返回创建的实体ID  


## DestroyVirtualEntity

**[服务端]**

method in Preset.Parts.EntityBasePart.EntityBasePart

  * 描述

移除已创建的关联实体，引擎退出时会默认调用

  * 参数

无

  * 返回值

无