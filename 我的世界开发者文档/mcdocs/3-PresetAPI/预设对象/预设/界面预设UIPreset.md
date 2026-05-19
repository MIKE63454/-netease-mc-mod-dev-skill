# 界面预设UIPreset

## 概述

  * 继承关系


  * 描述

UIPreset（界面预设）是一类绑定界面资源的预设。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
uiNodeScreen | str | UI画布路径，为"UI文件名.UI画布名"的字符串形式  
uiNodeModulePath | str | 继承自ScreenNode的UI逻辑文件模块路径  
uiNodeModule | str | 继承自ScreenNode的UI逻辑文件类名  
uiName | str | UI名称，需保证在单个addon中唯一  
autoCreate | bool | 在界面预设创建完成后是否自动创建UI  
isHud | bool | 该界面是否屏蔽游戏操作  
createUIMethod | str | 创建界面接口  
isBindParent | bool | 是否绑定父预设，仅当父预设为实体预设时生效  
bindParentOffset | tuple | 当绑定父预设时生效，修改与绑定实体之间的偏移量  
bindParentAutoScale | bool | 当绑定父预设时生效，设置已绑定实体的UI是否根据绑定实体与本地玩家间的距离动态缩放  
showInEditor | bool | 是否在预设编辑器中创建UI  


## 索引

接口 |  | 描述  
---|---|---  
[SetUiActive](<#setuiactive>) | **[客户端]** | 设置UI激活  
[GetUiActive](<#getuiactive>) | **[客户端]** | 获取当前UI是否激活  
[SetUiVisible](<#setuivisible>) | **[客户端]** | 设置UI显隐  
[GetUiVisible](<#getuivisible>) | **[客户端]** | 获取当前UI是否显示  
[GetScreenNode](<#getscreennode>) | **[客户端]** | 获取当前ScreenNode实例  
  
## SetUiActive

**[客户端]**

method in Preset.Model.UI.UIPreset.UIPreset

  * 描述

设置UI激活

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
active | bool | 是否激活  
  * 返回值

无


## GetUiActive

**[客户端]**

method in Preset.Model.UI.UIPreset.UIPreset

  * 描述

获取当前UI是否激活

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | UI是否激活  


## SetUiVisible

**[客户端]**

method in Preset.Model.UI.UIPreset.UIPreset

  * 描述

设置UI显隐

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
visible | bool | 是否显示  
  * 返回值

无


## GetUiVisible

**[客户端]**

method in Preset.Model.UI.UIPreset.UIPreset

  * 描述

获取当前UI是否显示

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | UI是否显示  


## GetScreenNode

**[客户端]**

method in Preset.Model.UI.UIPreset.UIPreset

  * 描述

获取当前ScreenNode实例

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
ScreenNode | 当前ScreenNode实例