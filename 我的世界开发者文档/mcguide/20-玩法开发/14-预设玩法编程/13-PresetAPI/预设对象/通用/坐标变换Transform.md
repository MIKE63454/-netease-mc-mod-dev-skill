# 坐标变换Transform

## 概述

  * 继承关系


  * 描述

坐标变换，包含位置、旋转和缩放

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
pos | tuple(float,float,float) | 位置变换  
rotation | tuple(float,float,float) | 旋转变换  
scale | tuple(float,float,float) | 缩放变换  


## 索引

接口 |  | 描述  
---|---|---  
[AddOffset](<#addoffset>) | **[客户端]**/**[服务端]** | 给坐标变换位置增加偏移量  
[AddRotation](<#addrotation>) | **[客户端]**/**[服务端]** | 给坐标变换旋转增加偏移量  
[AddScale](<#addscale>) | **[客户端]**/**[服务端]** | 给坐标变换缩放增加偏移量  
[AddTransform](<#addtransform>) | **[客户端]**/**[服务端]** | 给坐标变换增加偏移量  
[GetMatrix](<#getmatrix>) | **[客户端]**/**[服务端]** | 获取坐标变换矩阵  
  
## AddOffset

**[客户端]**/**[服务端]**

method in Preset.Model.Transform.Transform

  * 描述

给坐标变换位置增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
offset | tuple(float,float,float) | 变换位置  
  * 返回值

无


## AddRotation

**[客户端]**/**[服务端]**

method in Preset.Model.Transform.Transform

  * 描述

给坐标变换旋转增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
rotation | tuple(float,float,float) | 变换旋转  
  * 返回值

无


## AddScale

**[客户端]**/**[服务端]**

method in Preset.Model.Transform.Transform

  * 描述

给坐标变换缩放增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
scale | tuple(float,float,float) | 变换缩放  
  * 返回值

无


## AddTransform

**[客户端]**/**[服务端]**

method in Preset.Model.Transform.Transform

  * 描述

给坐标变换增加偏移量

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
transform | Transform | 坐标变换  
  * 返回值

无


## GetMatrix

**[客户端]**/**[服务端]**

method in Preset.Model.Transform.Transform

  * 描述

获取坐标变换矩阵

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
Matrix | 坐标变换矩阵