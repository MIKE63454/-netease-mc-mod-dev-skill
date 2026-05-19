# 变换对象TransformObject

## 概述

  * 继承关系


  * 描述

TransformObject（变换对象）是拥有变换属性的GameObject（游戏对象）的基类，他们在游戏世界中有着确切的位置等信息。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
name | str | 对象名称  
transform | Transform | 局部坐标变换  
dimension | Transform | 所在维度ID  
isBroken | bool | 是否可用，当素材文件丢失，零件代码语法错误时处于不可用状态  
isRemoved | bool | 是否已销毁  


## 索引

接口 |  | 描述  
---|---|---  
[GetDimension](<#getdimension>) | **[客户端]**/**[服务端]** | 获取所在的维度  
[SetDimension](<#setdimension>) | **[服务端]** | 设置所在的维度  
[GetChildTransformObjects](<#getchildtransformobjects>) | **[客户端]**/**[服务端]** | 获取子TransformObject列表  
[GetTransformObjects](<#gettransformobjects>) | **[客户端]**/**[服务端]** | 获取TransformObject列表，包含自身  
[GetChildGameObjects](<#getchildgameobjects>) | **[客户端]**/**[服务端]** | 获取子GameObject列表  
[GetGameObjects](<#getgameobjects>) | **[客户端]**/**[服务端]** | 获取GameObject列表，包含自身  
[GetGameObjectById](<#getgameobjectbyid>) | **[客户端]**/**[服务端]** | 根据ID获取GameObject  
[GetGameObjectByEntityId](<#getgameobjectbyentityid>) | **[客户端]**/**[服务端]** | 根据实体ID获取GameObject  
[GetId](<#getid>) | **[客户端]**/**[服务端]** | 获取当前预设的ID  
[GetEntityId](<#getentityid>) | **[客户端]**/**[服务端]** | 获取当前预设的实体ID  
[GetDisplayName](<#getdisplayname>) | **[客户端]**/**[服务端]** | 获取当前预设的显示名称  
[GetDisplayPath](<#getdisplaypath>) | **[客户端]**/**[服务端]** | 获取当前预设到根节点的显示路径  
[GetLocalTransform](<#getlocaltransform>) | **[客户端]**/**[服务端]** | 获取当前预设的局部坐标变换  
[SetLocalTransform](<#setlocaltransform>) | **[客户端]**/**[服务端]** | 设置当前预设的局部坐标变换  
[GetLocalPosition](<#getlocalposition>) | **[客户端]**/**[服务端]** | 获取当前预设的局部坐标位置  
[SetLocalPosition](<#setlocalposition>) | **[客户端]**/**[服务端]** | 设置当前预设的局部坐标位置  
[GetLocalRotation](<#getlocalrotation>) | **[客户端]**/**[服务端]** | 获取当前预设的局部坐标旋转  
[SetLocalRotation](<#setlocalrotation>) | **[客户端]**/**[服务端]** | 设置当前预设的局部坐标旋转  
[GetLocalScale](<#getlocalscale>) | **[客户端]**/**[服务端]** | 获取当前预设的局部坐标缩放  
[SetLocalScale](<#setlocalscale>) | **[客户端]**/**[服务端]** | 设置当前预设的局部坐标缩放  
[GetWorldTransform](<#getworldtransform>) | **[客户端]**/**[服务端]** | 获取当前预设的世界坐标变换  
[GetWorldMatrix](<#getworldmatrix>) | **[客户端]**/**[服务端]** | 获取世界坐标变换矩阵  
[GetLocalMatrix](<#getlocalmatrix>) | **[客户端]**/**[服务端]** | 获取局部坐标变换矩阵  
[SetWorldTransform](<#setworldtransform>) | **[客户端]**/**[服务端]** | 设置当前预设的世界坐标变换  
[GetWorldPosition](<#getworldposition>) | **[客户端]**/**[服务端]** | 获取当前预设的世界坐标位置  
[SetWorldPosition](<#setworldposition>) | **[客户端]**/**[服务端]** | 设置当前预设的世界坐标位置  
[GetWorldRotation](<#getworldrotation>) | **[客户端]**/**[服务端]** | 获取当前预设的世界坐标旋转  
[SetWorldRotation](<#setworldrotation>) | **[客户端]**/**[服务端]** | 设置当前预设的世界坐标旋转  
[GetWorldScale](<#getworldscale>) | **[客户端]**/**[服务端]** | 获取当前预设的世界坐标缩放  
[SetWorldScale](<#setworldscale>) | **[客户端]**/**[服务端]** | 设置当前预设的世界坐标缩放  
[AddLocalOffset](<#addlocaloffset>) | **[客户端]**/**[服务端]** | 给局部坐标变换位置增加偏移量  
[AddWorldOffset](<#addworldoffset>) | **[客户端]**/**[服务端]** | 给世界坐标变换位置增加偏移量  
[AddLocalRotation](<#addlocalrotation>) | **[客户端]**/**[服务端]** | 给局部坐标变换旋转增加偏移量  
[AddWorldRotation](<#addworldrotation>) | **[客户端]**/**[服务端]** | 给世界坐标变换旋转增加偏移量  
[AddLocalScale](<#addlocalscale>) | **[客户端]**/**[服务端]** | 给局部坐标变换缩放增加偏移量  
[AddWorldScale](<#addworldscale>) | **[客户端]**/**[服务端]** | 给世界坐标变换缩放增加偏移量  
[AddLocalTransform](<#addlocaltransform>) | **[客户端]**/**[服务端]** | 给局部坐标变换增加偏移量  
[AddWorldTransform](<#addworldtransform>) | **[客户端]**/**[服务端]** | 给世界坐标变换增加偏移量  
[GetRootParent](<#getrootparent>) | **[客户端]**/**[服务端]** | 获取当前预设所在的根预设  
[GetParent](<#getparent>) | **[客户端]**/**[服务端]** | 获取当前预设的父预设  
[SetParent](<#setparent>) | **[客户端]**/**[服务端]** | 设置当前预设的父预设  
[GetManager](<#getmanager>) | **[客户端]**/**[服务端]** | 获取当前预设所在的预设管理器  
[Unload](<#unload>) | **[客户端]**/**[服务端]** | 卸载当前预设  
[Destroy](<#destroy>) | **[客户端]**/**[服务端]** | 销毁当前预设  
  
## GetDimension

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取所在的维度

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 维度ID  


## SetDimension

**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置所在的维度

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
dimension | int | 目标维度ID  
pos | tuple(int,int,int) | 传送的坐标，默认为空，非空时会更新该对象的世界坐标  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否真正变更了维度，如果目标维度与对象维度一致，会返回False  


## GetChildTransformObjects

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取子TransformObject列表

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
recursive | bool | 是否递归查找所有子节点  
  * 返回值

数据类型 | 说明  
---|---  
list(TransformObject) | TransformObject列表  


## GetTransformObjects

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取TransformObject列表，包含自身

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
recursive | bool | 是否递归查找所有子节点  
  * 返回值

数据类型 | 说明  
---|---  
list(TransformObject) | TransformObject列表  


## GetChildGameObjects

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取子GameObject列表

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
recursive | bool | 是否递归查找所有子节点  
  * 返回值

数据类型 | 说明  
---|---  
list(GameObject) | 游戏对象列表  


## GetGameObjects

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取GameObject列表，包含自身

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
recursive | bool | 是否递归查找所有子节点  
  * 返回值

数据类型 | 说明  
---|---  
list(GameObject) | 游戏对象列表  


## GetGameObjectById

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

根据ID获取GameObject

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
GameObject | 游戏对象  


## GetGameObjectByEntityId

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

根据实体ID获取GameObject

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
GameObject | 游戏对象  


## GetId

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的ID

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | ID  


## GetEntityId

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的实体ID

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | 实体ID  


## GetDisplayName

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的显示名称

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | 名称  


## GetDisplayPath

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设到根节点的显示路径

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | 节点路径  


## GetLocalTransform

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的局部坐标变换

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
Transform | 坐标变换  


## SetLocalTransform

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的局部坐标变换

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
transform | Transform | 坐标变换  
  * 返回值

无


## GetLocalPosition

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的局部坐标位置

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(float,float,float) | （X轴位置，Y轴位置，Z轴位置）  


## SetLocalPosition

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的局部坐标位置

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
pos | tuple(float,float,float) | （X轴位置，Y轴位置，Z轴位置）  
  * 返回值

无


## GetLocalRotation

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的局部坐标旋转

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(float,float,float) | （X轴角度，Y轴角度，Z轴角度）  


## SetLocalRotation

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的局部坐标旋转

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
rotation | tuple(float,float,float) | （X轴角度，Y轴角度，Z轴角度）  
  * 返回值

无


## GetLocalScale

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的局部坐标缩放

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(float,float,float) | （X轴缩放，Y轴缩放，Z轴缩放）  


## SetLocalScale

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的局部坐标缩放

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
scale | tuple(float,float,float) | （X轴缩放，Y轴缩放，Z轴缩放）  
  * 返回值

无


## GetWorldTransform

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的世界坐标变换

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
Transform | 坐标变换  


## GetWorldMatrix

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取世界坐标变换矩阵

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
Matrix | 世界坐标变换矩阵  


## GetLocalMatrix

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取局部坐标变换矩阵

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
Matrix | 局部坐标变换矩阵  


## SetWorldTransform

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的世界坐标变换

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
transform | Transform | 坐标变换  
  * 返回值

无


## GetWorldPosition

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的世界坐标位置

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(float,float,float) | （X轴位置，Y轴位置，Z轴位置）  


## SetWorldPosition

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的世界坐标位置

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
pos | tuple(float,float,float) | （X轴位置，Y轴位置，Z轴位置）  
  * 返回值

无


## GetWorldRotation

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的世界坐标旋转

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(float,float,float) | （X轴角度，Y轴角度，Z轴角度）  


## SetWorldRotation

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的世界坐标旋转

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
rotation | tuple(float,float,float) | （X轴角度，Y轴角度，Z轴角度）  
  * 返回值

无


## GetWorldScale

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的世界坐标缩放

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(float,float,float) | （X轴缩放，Y轴缩放，Z轴缩放）  


## SetWorldScale

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的世界坐标缩放

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
scale | tuple(float,float,float) | （X轴缩放，Y轴缩放，Z轴缩放）  
  * 返回值

无


## AddLocalOffset

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给局部坐标变换位置增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
offset | tuple(float,float,float) | 变换位置  
  * 返回值

无


## AddWorldOffset

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给世界坐标变换位置增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
offset | tuple(float,float,float) | 变换位置  
  * 返回值

无


## AddLocalRotation

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给局部坐标变换旋转增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
rotation | tuple(float,float,float) | 变换旋转  
  * 返回值

无


## AddWorldRotation

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给世界坐标变换旋转增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
rotation | tuple(float,float,float) | 变换旋转  
  * 返回值

无


## AddLocalScale

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给局部坐标变换缩放增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
scale | tuple(float,float,float) | 变换缩放  
  * 返回值

无


## AddWorldScale

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给世界坐标变换缩放增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
scale | tuple(float,float,float) | 变换缩放  
  * 返回值

无


## AddLocalTransform

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给局部坐标变换增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
transform | Transform | 坐标变换  
  * 返回值

无


## AddWorldTransform

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

给世界坐标变换增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
transform | Transform | 坐标变换  
  * 返回值

无


## GetRootParent

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设所在的根预设

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
PresetBase | 预设  


## GetParent

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设的父预设

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
PresetBase | 预设  


## SetParent

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

设置当前预设的父预设

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
parent | PresetBase | 预设  
  * 返回值

无


## GetManager

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

获取当前预设所在的预设管理器

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
PresetManager | 预设管理  


## Unload

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

卸载当前预设

  * 参数

无

  * 返回值

无


## Destroy

**[客户端]**/**[服务端]**

method in Preset.Model.TransformObject.TransformObject

  * 描述

销毁当前预设

  * 参数

无

  * 返回值

无