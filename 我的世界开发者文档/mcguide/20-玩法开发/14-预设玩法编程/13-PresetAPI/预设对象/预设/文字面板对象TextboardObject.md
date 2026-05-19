# 文字面板对象TextboardObject

## 概述

  * 继承关系


  * 描述

TextboardObject(文字面板对象)是对文字面板对象封装的基类，为文字面板提供了面向对象方法

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
textboardId | int | 关联文字面板ID  


## 索引

接口 |  | 描述  
---|---|---  
[SetBindEntity](<#setbindentity>) | **[客户端]** | 文字面板绑定实体对象  
[SetPos](<#setpos>) | **[客户端]** | 修改文字面板预设位置  
[SetRot](<#setrot>) | **[客户端]** | 修改旋转角度, 若设置了文本朝向相机，则旋转角度的修改不会生效  
[SetScale](<#setscale>) | **[客户端]** | 内容整体缩放  
[SetText](<#settext>) | **[客户端]** | 修改文字面板内容  
[SetColor](<#setcolor>) | **[客户端]** | 修改字体颜色  
[SetBackgroundColor](<#setbackgroundcolor>) | **[客户端]** | 修改背景颜色  
[SetFaceCamera](<#setfacecamera>) | **[客户端]** | 设置是否始终朝向相机  
[SetBoardDepthTest](<#setboarddepthtest>) | **[客户端]** | 设置是否开启深度测试, 默认状态下是开启  
  
## SetBindEntity

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

文字面板绑定实体对象

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
bindEntityId | str | 绑定entity的Id; 如果为None，则为取消实体绑定, 此时下面参数为世界坐标和旋转  
offset | tuple(float,float,float) | 相对于实体的偏移量  
rot | tuple(float,float,float) | 相对于实体的偏移角度  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetBindEntity(self.GetLocalPlayerId(), (0.0, 1.5, 0.0), (0.0, 0.0, 0.0))
```
## SetPos

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

修改文字面板预设位置

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
pos | tuple(float,float,float) | 坐标  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetPos((0.0, 3.0, 0.0))
```
## SetRot

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

修改旋转角度, 若设置了文本朝向相机，则旋转角度的修改不会生效

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
rot | tuple(float,float,float) | 角度(不是弧度)  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetRot((45.0, 90.0, 0.0))
```
## SetScale

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

内容整体缩放

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
scale | tuple(float,float) | x,y方向上的缩放值,要求值大于0,正常状态下是(1.0,1.0)  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetScale((2.0, 2.0))
```
## SetText

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

修改文字面板内容

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
text | str | 文字内容  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否修改成功  
  * 示例
```python
    self.SetText("修改后的文字")
```
## SetColor

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

修改字体颜色

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
textColor | tuple(float,float,float,float) | 颜色的RGBA值，范围0-1  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetColor((1.0, 1.0, 0.0, 0.8))
```
## SetBackgroundColor

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

修改背景颜色

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
backgroundColor | tuple(float,float,float,float) | 颜色的RGBA值，范围0-1  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetBackgroundColor((1.0, 1.0, 1.0, 1.0))
```
## SetFaceCamera

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

设置是否始终朝向相机

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
faceCamera | bool | 是否始终朝向相机, 默认为True  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetFaceCamera(True)
```
## SetBoardDepthTest

**[客户端]**

method in Preset.Model.Textboard.TextboardObject.TextboardObject

  * 描述

设置是否开启深度测试, 默认状态下是开启

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
depthTest | bool | True为开启深度测试,False为不开启  
  * 返回值

数据类型 | 说明  
---|---  
bool | 返回是否设置成功  
  * 示例
```python
    self.SetBoardDepthTest(False)
```