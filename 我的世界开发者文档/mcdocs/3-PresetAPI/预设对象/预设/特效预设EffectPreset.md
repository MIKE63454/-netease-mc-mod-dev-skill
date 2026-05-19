# 特效预设EffectPreset

## 概述

  * 继承关系


  * 描述

EffectPreset（特效预设）是一类绑定特效资源的预设。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
resource | str | 特效json的相对路径，不含后缀.json  
effectType | str | 特效类型，frame/particle  
effectId | int | 特效ID  
auto | bool | 是否自动播放  


## 索引

接口 |  | 描述  
---|---|---  
[GetResource](<#getresource>) | **[客户端]** | 获取绑定的json资源  
[SetResource](<#setresource>) | **[客户端]** | 设置绑定的json资源  
  
## GetResource

**[客户端]**

method in Preset.Model.Effect.EffectPreset.EffectPreset

  * 描述

获取绑定的json资源

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | json资源相对路径，不含.json后缀  


## SetResource

**[客户端]**

method in Preset.Model.Effect.EffectPreset.EffectPreset

  * 描述

设置绑定的json资源

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
resource | str | json资源相对路径，不含.json后缀  
  * 返回值

无