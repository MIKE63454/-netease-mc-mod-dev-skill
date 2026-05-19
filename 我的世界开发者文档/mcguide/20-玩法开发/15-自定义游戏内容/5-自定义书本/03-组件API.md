# 组件API

## 1.BaseComp

所有书本组件都继承**BaseComp** ，本质将数据和UI控件对象封装在一起，通过程序逻辑将数据正确地填充到对应的UI控件中，而UI控件的获取是从[UI模板库](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#UI模板库>)中获取的。整体结构如下所示。

![structure](/dev/mcmanual/mc-dev/assets/img/structure.ac8dfd66.png)

### 1.重写方法

#### __init__

  * 描述

初始化组件。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
compName | str | 注册的自定义组件的名称，为了防止重名，建议名字格式为 'mod名称:组件名称'。  
jsonFile | str | 组件所封装的UI控件所在的json文件名（无后缀） +'.main'。  
compNodeName | str | 组件所封装的UI控件节点名称，该节点作为组件的UI根节点（RootUINode）。  
recycled | bool | 组件是否可回收，详见备注。  
默认值为False  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 当你自定义一个组件的时候**必须重写类初始化方法** ，并执行父类"**__init__** "方法，如何重写请参考[脚本自定义书本](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义组件>)。
    * 自定义书本第一次显示组件的时候会从UI模板库中拷贝对应的UI控件节点到书本界面并显示出来，**如果开启组件可回收** ，那么在组件不显示的时候（比如翻页的时候第一页的组件就会调用Hide方法隐藏），**拷贝出来的UI控件节点不会被删除** ，而会被加入到空闲节点队列中，当**该组件第二次显示** 或者有**别的同名组件要显示** 的时候就会从空闲队列中寻找可用的UI控件节点，从而避免过多的删除及拷贝。但是因为节点复用的原因，如果第一次对UI控件节点进行属性更改，那么复用的时候必须考虑哪些属性需要重置，所以**如果开启了组件可回收，必须重写Reset方法** ，组件在调用Hide前会调用Reset。
    * **如果关闭组件可回收** ，那么组件在不显示的时候，拷贝出来的UI控件节点会被**立即删除** ，不会加入到空闲节点队列中，当组件再次显示的时候，会**重新** 从UI模板库中**拷贝** 对应的UI控件节点显示，因此**无需考虑属性重置，无需重写Reset方法** 。
    * 因为拷贝UI节点存在性能开销，开启组件回收能一定程度提高UI渲染的速度，但是也增加了内存占用，增加了程序逻辑控制。
    * 一个组件只能对应一个**UI控件根节点** ，组件调用父类"**__init__** "方法时就会为这个组件注册名称**compName** （内部有防止重复注册），组件每次需要显示的时候就依照**compName** 从UI模板库中获取对应的UI控件根节点。


#### Show

  * 描述

显示组件。

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 当你自定义一个组件的时候**必须重写该方法** ，并执行父类**Show** 方法，必须返回自身以支持链式调用。如何重写请参考[脚本自定义书本](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义组件>)。
    * 该方法的调用时机：该方法由页类进行调用，页类在对组件排版之前会调用自身的**Show** 方法，从而调用组件的**Show** 方法。
    * 组件只有显示出来才会对UI控件节点进行拷贝显示，所以组件未显示之前无法获取其封装的UI控件节点。
    * 通常是在该方法中将数据设置到UI控件节点中。


#### Hide

  * 描述

隐藏组件。

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 当你自定义一个组件的时候**可以不重写该方法** ，但如果重写该方法一定要在最后主动调用父类**Hide** 方法，必须返回自身以支持链式调用。
    * 该方法的调用时机：该方法由页类进行调用，页类在隐藏的时候会调用自身的**Hide** 方法，从而调用组件的**Hide** 方法。
    * 如果组件可回收，调用该方法会调用组件的**Reset** 方法，对组件内的UI控件节点进行属性重置。如果组件不可回收，则会直接删除其内的UI控件节点，下次显示的时候重新从UI模板库中拷贝。
  * 示例
[code] def Hide(self):
            """
            	这是一个继承BaseComp的组件类的重写示例，比如我们希望组件隐藏前先将组件中的数据清空，可以如下编写
            """
            # 清空组件内的数据
            self.clearData()
            # 只需要在前面完成别的逻辑，最后调用父类方法即可。
           	return BaseComp.Hide(self)
```
#### Reset

  * 描述

当组件回收的时候对UI控件节点进行属性重置。

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 当你自定义一个组件的时候**可以不重写该方法** ，但如果该组件的类型为**可回收** ，建议重写该方法，在该方法中**无需调用父类Reset方法** 。
    * 该方法的调用时机：该方法由组件调用，组件调用Hide的时候会先调用该方法。
    * 如果Show方法中对UI控件节点默认属性（UI json中定义）进行修改了，并且组件希望每次Show的时候都希望读取默认值，则需要在该方法中填充逻辑，否则只需要直接返回self。
  * 示例

为了说明Reset函数重写的必要性，这里以一个错误例子为例，该例子中UI控件节点的结构如下：
[code] | RootUINode		# 组件封装的控件节点（根节点）
        	| entity		# 布娃娃控件
        	| background	# 图片控件
```
我们希望在组件中提供一个偏移属性，让 entity 节点相对于自己的默认位置（UI Json中布局）往上移动3px。
[code] def Show(self):
            # 获取 entity 节点
            entityNode = self.GetRootUINode().GetChildByPath("/entity").asNeteasePaperDoll()
            # 读取当前 entity 位置
            pos = entityNode.GetPosition()
            newPos = (pos[0], pos[1] - 3)
            entityNode.SetPosition(newPos)
```
Show方法偏移之前是需要获取该节点的位置的，每次的Show就会导致该节点偏移叠加起来，最终的结果看起来就会如下图所示（组件逐渐上移）。

![Reset错误示例](/dev/mcmanual/mc-dev/assets/img/Reset错误示例.b0fe526d.gif)

为了解决UI控件复用导致的问题，可以使用Reset的方法对该类进行属性重置，让每次读取得到的pos都是与UI json中定义的默认值一致。
[code] def Show(self):
            # 获取 entity 节点
            entityNode = self.GetRootUINode().GetChildByPath("/entity").asNeteasePaperDoll()
            # 读取当前 entity 位置
            pos = entityNode.GetPosition()
            newPos = (pos[0], pos[1] - 3)
            entityNode.SetPosition(newPos)
            # 记录下位置
            self.originPos = pos
        
        def Reset(self):
            entityNode = self.GetRootUINode().GetChildByPath("/entity").asNeteasePaperDoll()
            # 恢复位置属性
            entityNode.SetPosition(self.originPos)
```
该方法为统一接口，主要是**提示开发者使用组件复用的时候需要留意的问题** ，开发者完全可以通过别的方式实现属性的重置（比如第一次读取就将默认属性存储下来然后后面show的时候读取）而**不重写Reset方法** 。

比如我们的Demo中的一个自定义组件**MyRecyleToggle** ，具体可见示例[CustomBookMod](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/13-模组SDK编程/60-Demo示例.html#CustomBookMod>)中的“**behavior_pack/tutorialScripts/comps/recycleToggleComp.py”**以及包含它的自定义页面“**behavior_pack/tutorialScripts/pages/recyclePage.py”**和** json**数据“**behavior_pack/customBooks/RecycleBook”**中的用法（该书本对应的物品中文名称为：“**回收组件测试书本** ”），这个组件的**Reset**方法并没有具体实现，而是自己组件记录下状态，只从**json**数据中读取第一次数据。

另外，为了方便开发者，一些常见的复用问题已经内置工具方法解决，比如关于节点的偏移导致的控件复用问题，可以使用**BaseComp** 中提供工具的方法 **SetNodeOffset** 解决，更多的可见下面的方法。


### 2.排版UI控件的方法

BaseComp提供了一系列方法方便开发者在页面中进行排版，这些方法依照的UI坐标系参见[“自定义书本UI坐标系”](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/01-自定义基础书本.html#页面编写>)。在对组件调用排版方法之前，需确保组件处于**显示状态** ，也就是组件已经获取到封装的**UI控件对象** 。

对组件进行排版排版实际上就是操作组件所封装的**控件根节点** ，最基本的四个方法就是**GetPosition，SetPosition，GetSize，SetSize** 。其余方法本质是调用这四个方法，比如获取组件左右边界，上下边界以及中心的**Left，Right，Top，Bottom，Center** ，以及各类将组件与别的东西对齐的方法（以**Align** 为前缀的方法），以**Move** 为前缀的方法，这些方法在使用之前必须先调用**SetSize** ，否则组件排版时会产生意想不到的结果。

#### GetPosition

  * 描述

获取组件在书本坐标系中的位置。

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 组件在书本坐标系中的位置，单位为像素  
  * 备注

    * 组件的根节点控件的**锚点** 是位于**左上角** 的，具体可见[自定义组件](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义组件>)，所以此处获取得到的坐标也是指锚点在书本坐标系中的坐标，参见[书本坐标系](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/01-自定义基础书本.html#页面编写>)。


#### SetPosition

  * 描述

设置组件在书本坐标系中的位置。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
newPosition | tuple(int, int) | 组件在书本坐标系中的位置，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 该方法实际上调用的是组件中的根节点控件的**SetPosition** 方法。


#### GetSize

  * 描述

获取组件的大小

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 组件的大小，单位为像素。  
  * 备注

    * 该方法实际上调用的是组件中的根节点控件的**GetSize** 方法。


#### SetSize

  * 描述

设置组件大小

参数名 | 数据类型 | 说明  
---|---|---  
newSize | tuple(int, int) | 设置的组件大小，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 该方法实际上调用的是组件中的根节点控件的**SetSize** 方法，如果该根节点下面的节点控件均为自适应的布局结构（比如大小定义为**跟随父控件百分比** ），那么改变组件大小的同时根节点下的自适应节点控件也会跟着改变。而如果根节点下面的非自适应布局的节点控件，则不会改变其固定的大小。
    * 如果开发者不满足于UI所提供的一系列控件布局方法，则建议重写本方法定义自己的控件缩放规则。


#### Center

  * 描述

获取组件的中心坐标

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 组件的中心坐标，单位为像素。  


#### Left

  * 描述

获取组件左边界的X值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 组件左边界的X值，单位为像素。  


#### Right

  * 描述

获取组件右边界的X值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 组件右边界的X值，单位为像素。  


#### Top

  * 描述

获取组件上边界的Y值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 组件上边界的Y值，单位为像素。  


#### Bottom

  * 描述

获取组件下边界的Y值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 组件下边界的Y值，单位为像素。  


#### MoveToX

  * 描述

不改变组件位置坐标的Y值，单独设置组件位置坐标的X值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | 设置坐标的x值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### MoveToY

  * 描述

不改变组件位置坐标的X值，单独设置组件位置坐标的Y值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
y | int | 设置坐标的y值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### MoveX

  * 描述

沿X轴正方向移动一段距离

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | 移动的距离，如果为负数，则表示往负方向移动，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### MoveY

  * 描述

沿Y轴正方向移动一段距离

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
y | int | 移动的距离，如果为负数，则表示往负方向移动，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignCenterToX

  * 描述

水平移动组件使组件中心到指定的x值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | 指定的x值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignCenterToY

  * 描述

垂直移动组件使组件中心到指定的y值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | 指定的x值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignCenterToPosition

  * 描述

移动组件使组件中心到指定的坐标值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
position | tuple(int, int) | 指定的坐标值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignLeftToX

  * 描述

水平移动组件使组件左边界到指定的x值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | 指定的x值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignRightToX

  * 描述

水平移动组件使组件右边界到指定的x值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
x | int | 指定的x值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignTopToY

  * 描述

垂直移动组件使组件上边界到指定的y值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
y | int | 指定的y值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### AlignBottomToY

  * 描述

垂直移动组件使组件下边界到指定的y值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
y | int | 指定的y值，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


### 3.与UI控件节点相关的方法

#### GetRootUINode

  * 描述

获取组件所封装的UI控件根节点

  * 参数

无

  * 备注

    * 仅当组件处于可见状态的时候该方法才会返回BaseUIControl类型的根节点
    * 如果组件不可见，则会报错并返回None
  * 返回值

数据类型 | 说明  
---|---  
BaseUIControl | 组件所封装的UI控件根节点  


#### HasRootUINode

  * 描述

判断组件是否拥有UI控件根节点

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | 如果组件拥有UI控件根节点则返回True，否则返回False  


#### SetNodeOffset

  * 描述

对组件所封装的UI控件节点（以及其子节点）进行位置偏移。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
node | BaseUIControl | 要进行位置偏移的节点  
offset | tuple(int, int) | 偏移值，单位为像素。  
  * 备注

    * 该方法主要是为了解决组件中UI控件节点复用的问题，调用该方法时会记录下节点偏移之前的位置，然后在组件隐藏的时候复原节点的位置，保证下次组件显示的时候仍然是正确的，因此用户无需在Reset中重置对应节点的位置。
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 示例

仍然以**Reset** 函数的示例说明，组件中的UI控件节点结构如下所示：
[code] | RootUINode		# 组件封装的控件节点（根节点）
        	| entity		# 布娃娃控件
        	| background	# 图片控件
```
在显示的时候将 entity 节点相对于自己的默认位置（UI Json中布局）往上移动3px。
[code] def Show(self):
            # 获取 entity 节点
            entityNode = self.GetRootUINode().GetChildByPath("/entity").asNeteasePaperDoll()
            # 偏移 entity 节点
            self.SetNodeOffset(entityNode, (0, -3))
```
并且无需再重写**Reset** 函数。


#### SetNodeSize

  * 描述

对组件所封装的UI控件节点（以及其子孙节点）进行大小设置。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
node | BaseUIControl | 要进行位置偏移的节点  
newSize | tuple(int, int) | 要设置的节点大小  
  * 备注

    * 该方法类似**SetNodeOffset** 的作用，主要是为了解决组件中UI控件节点复用的问题，用户无需在Reset中重置对应节点的大小。
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  


#### SetNodeText

  * 描述

对组件中指定的文本控件（LabelUIControl）设置文本。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
node | BaseUIControl | 要设置的文本节点  
text | str | 要设置的文本值  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 相比于获取**LabelUIControl** 控件节点后直接调用其**SetText** 方法，该方法会在设置文本后刷新界面。
    * 该方法结合组件回收的时候，需要在Reset中重置文本。
  * 示例

比如组件中的UI控件节点结构如下所示：
[code] | RootUINode	# 组件封装的控件节点（根节点）
        	| somepanel	# panel
            	| text	# 文本控件（LabelUIControl）
```
在显示的时候修改"**text "控件节点**的文本为"**hello world** "：
[code] def Show(self):
            # 获取 文本 节点
            textNode = self.GetRootUINode().GetChildByPath("/somepanel/text")
            # 设置文本
            self.SetNodeText(textNode, "hello world")
```
#### SetNodeTextFontSize

  * 描述

对组件中指定的文本控件（LabelUIControl）设置其文字大小

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
node | BaseUIControl | 要设置的文本节点  
originFontSize | int | 文字原来的大小（UI json中定义的默认值），单位为像素。  
newFontSize | int | 要设置的文字大小，单位为像素。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 该方法结合组件回收的时候，需要在Reset中重置字体大小值。
  * 示例

比如组件中的UI控件节点结构如下所示：
[code] | RootUINode	# 组件封装的控件节点（根节点）
        	| somepanel	# panel
            	| text	# 文本控件（LabelUIControl）
```
"**text** "控件节点的原本的字号为8，在显示的时候修改为10：
[code] def Show(self):
            # 获取 文本 节点
            textNode = self.GetRootUINode().GetChildByPath("/somepanel/text")
            # 设置文本
            self.SetNodeTextFontSize(textNode, 8, 10)
```
#### GetNodeCenterGlobal

  * 描述

获取组件中的指定UI控件节点的中心全局坐标

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
node | BaseUIControl | 指定的UI控件节点  
  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 指定UI控件节点的中心全局坐标  
  * 备注

    * 全局坐标系与书本坐标系不同，详见[全局坐标系](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/01-自定义基础书本.html#页面编写>)


#### SetLayer

  * 描述

更改组件所封装UI控件根节点的UI层级

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
layer | int | 要设置的UI层级  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 该方法结合组件回收的时候，需要在Reset中重置UI层级。


### 4.其他工具方法

#### Call

  * 描述

调用回调函数

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
callbackDict | dict | 回调函数以及其参数（属性见备注）  
  * 返回值

数据类型 | 说明  
---|---  
object | 回调函数的返回值  
  * 备注

    * 该函数主要用于将回调函数作为额外的数据传进到组件中（比如**ButtonComp** ）

    * callbackDict的格式如下：

参数名 | 数据类型 | 说明  
---|---|---  
func | function | 回调的函数  
args | list | 回调函数的参数列表，如果回调函数无参，则该属性无需定义  
  * 示例
[code] """
        	我们定义两个回调函数，然后分别使用 Call 回调它
        """
        def func_1():
            pass
        def func_2(args1, args2):
            pass
        
        callbackDict_1 = {
            "func": func_1	# func_1函数没有参数，所以这里不传入参数列表
        }
        callbackDict_2 = {
            "func": func_2,
            "args": [1, 2]	# func_2函数需要两个参数，所以这里传入的是包含两个参数的参数列表
        }
        
        comp = BaseComp()
        comp.Call(callbackDict_1)
        comp.Call(callbackDict_2)
```
#### GetPage

  * 描述

返回组件当前所在的页面对象

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
[BasePage](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/04-页面API.html#BasePage>) | 页面实例  
如果组件未被加入到任何页面中，则返回None  


## 2.系统预设控件

所有的预设控件均继承自**BaseComp** ，在显示组件之前一定要先设置组件的数据。

### 1.TextComp

#### __init__

  * 描述

文本组件初始化

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
textAlign | int | 文本的对齐方式，可选值为以下值，备注中会图文说明：  
BookConfig.TextAlign.Left  
BookConfig.TextAlign.Right  
BookConfig.TextAlign.Center  
BookConfig.TextAlign.Fit_Left  
BookConfig.TextAlign.Fit_Center  
BookConfig.TextAlign.Fit_Right  
默认值为：BookConfig.TextAlign.Left  
  * 返回值

数据类型 | 说明  
---|---  
TextComp | 组件实例  
  * 备注

![textComp对齐方式](/dev/mcmanual/mc-dev/assets/img/textComp对齐方式.a058af52.png)

**Fit前缀** 的表示UI控件**根节点** 的大小会自适应文本内容（**红色框** 为UI控件根节点的实际边框），所以**Fit** 类型的文本组件调用**SetSize** 方法是**无效** 的。需注意，文本框的对齐方式和其所在的组件的实际对齐是没关系的，组件的对齐是需要调用各种Align方法实现的，对齐的对象是UI控件根节点，而文本框的对齐表示的是框内对齐方式（并且**框内是不支持垂直居中** 的）。

  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取书本配置常量，详细API见“05-常见脚本对象”
        bcf = bookManager.GetBookConfig()
        # 获取预设组件类 TextComp
        TextComp = bookManager.GetTextCompCls()
        # 创建TextComp实例
        textComp = TextComp(bcf.TextAlign.Fit_Center)
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
text | str | 文本内容  
textSize | int | 文本内容的字体大小，单位为像素  
默认值为**BookConfig.TextSize.content** ，数值为**10**  
  * 返回值

数据类型 | 说明  
---|---  
TextComp | 组件实例  
  * 备注

    * 该函数应该在组件显示之前被调用（也就是在组件调用**Show** 之前）
  * 示例
[code] textComp.SetDataBeforeShow("hello world", 12)	# 内容为"hello world"，字体大小为12px
        textComp.Show()
```
#### SetAlpha

  * 描述

设置文本字体透明度

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
alpha | float | 透明度值，范围都在(0, 1)内  
  * 返回值

数据类型 | 说明  
---|---  
TextComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用控件节点的**SetAlpha** 。
  * 示例
[code] textComp.SetAlpha(0.5)	# 设置文本半透明
```
#### SetColor

  * 描述

设置文本字体颜色

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
color | tuple(float, float, float, float) | 颜色值，分别为RGBA四个通道值，范围都在(0, 1)内  
  * 返回值

数据类型 | 说明  
---|---  
TextComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用控件节点的**SetColor** 。
  * 示例
[code] textComp.SetColor((0, 0, 1.0, 1.0))	# 设置文本颜色为蓝色
```
### 2.ImageComp

#### __init__

  * 描述

图片组件初始化

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
imageResizeRule | int | 图片的大小缩放规则，可选值为以下值，备注中会图文说明：  
BookConfig.ImageReszieRule.Ninesliced  
BookConfig.ImageReszieRule.Fit  
默认值为**BookConfig.ImageReszieRule.Ninesliced**  
  * 返回值

数据类型 | 说明  
---|---  
ImageComp | 组件实例  
  * 备注

![图片缩放规则](/dev/mcmanual/mc-dev/assets/img/图片缩放规则.7d4ab278.png)

**Fit** 表示图片保持固定长宽比，而**Ninesliced** 会改变原有图片的长宽比，使之适应给定的大小。

  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取书本配置常量，详细API见“05-常见脚本对象”
        bcf = bookManager.GetBookConfig()
        # 获取预设组件类 ImageComp
        ImageComp = bookManager.GetImageCompCls()
        # 创建ImageComp实例
        imageComp = ImageComp(BookConfig.ImageReszieRule.Fit)	# 该图片组件的缩放规则为固定长宽比
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
image | str | 显示的图片路径，这里写的是路径字符串，比如要索引”**resource_pack/textures/items/apple.png** “这一图片 就写地址”**textures/items/apple** “。  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 该函数应该在组件显示之前被调用（也就是在组件调用**Show** 之前）
  * 示例
[code] imageComp.SetDataBeforeShow(“textures/items/apple”)	# 图片显示为苹果
        imageComp.Show()
```
#### SetAlpha

  * 描述

设置图片透明度

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
alpha | float | 透明度值，范围都在(0, 1)内  
  * 返回值

数据类型 | 说明  
---|---  
BaseComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用控件节点的**SetAlpha** 。
  * 示例
[code] imageComp.SetAlpha(0.5)	# 设置图片半透明
```
### 3.HighlightComp

#### __init__

  * 描述

轮播物品组件初始化

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
HighlightComp | 组件实例  
  * 备注

![highlightComp](/dev/mcmanual/mc-dev/assets/img/highlightComp.0082a0d1.gif)

该组件每隔3秒会播放下一个物品，点击能显示物品信息。

  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取预设组件类 HighlightComp
        HighlightComp = bookManager.GetHighlightCompCls()
        # 创建HighlightComp实例
        highlightComp = HighlightComp()
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
itemData | dict | 需要轮播的物品数据，格式详见备注  
  * 返回值

数据类型 | 说明  
---|---  
HighlightComp | 组件实例  
  * 备注

    * 该函数应该在组件显示之前被调用（也就是在组件调用**Show** 之前）

    * itemData为**List** 类型，存储的每个元素的格式如下：

参数名 | 数据类型 | 说明  
---|---|---  
item | str | 物品的**identifier** ，支持[自定义物品](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/1-自定义物品/1-自定义基础物品.html>)  
data | int | 物品的附加值**AuxValue** ，该属性可以不写，默认值为0。  
  * 示例
[code] itemData = [
            {
                "item": "minecraft:iron_pickaxe",
                "data": 0
            },
            {
                "item": "minecraft:iron_shovel",
                "data": 0
            },
            {
                "item": "minecraft:iron_sword",
                "data": 0
            }
        ]
        highlightComp.SetDataBeforeShow(itemData)	# 设置了3个物品的轮播
        highlightComp.Show()
```
结果如下图所示：

![highlightComp2](/dev/mcmanual/mc-dev/assets/img/highlightComp2.0a52eb94.gif)


### 3.TableRecipeComp

#### __init__

  * 描述

工作台合成表组件初始化

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
TableRecipeComp | 组件实例  
  * 备注

![tableRecipe1](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASEAAACjCAYAAADFEqh8AAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAA6YSURBVHhe7d2/q2xXGcbx/E8SEMTeSgheBbFIJYiiRRC8jQhp/QESDAHBRpQUNmIVDJax8EcXiKCEgLFIJYi20TXJI+9d+c7ea+9Za/Z+1zwDH/DMOTPznLXe9yH3BuJzf3/rxx+uef/3r5iZDeESMrNDPff+W//7H2vghWZmPbiEzOxQLiEzO5RLyMwO5RIys0O5hMzsUC4hMzuUS8jMDtVUQn958/tmZptQ4RCXkJkNQYVDXEJmNgQVDnEJmdkQVDjkOXqyRh9gZraEuoS4hMxsCOoS4hIysyGoS8iwEnr5m58zewbNic2LuoS4hOxuaE5sXtQl5LQl9Pzzz29G73MvlGcNvc+9UJ419D5b0JzYvKhLyF1K6M2fP92MlmANvc+a/7z3u/9z3mX0PmtiXpoTmxd1CTlNCdUD//Tp00947bXXFtXvQZ9Ti0vSMy/lq9XvQZ9Ty5yX5sTmRV1CXEJhSbItdba8NCc2L+oScngJaaC1DL/649sXf/vXvy+WHvoZ0Wv1Xi3LEpfEeZ99xKzFrXlpTmxe1CXEJRSWxHmffcSsxa15aU5sXtQl5DQlpAHv8dB7bV0S52177M1Lc2Lzoi4hLqGwJM7b9tibl+bE5kVdQk5TQjUNev1HgqJ+1EtRo8+VuCQj8tLjzHkpc6+8NCc2L+oS4hIKSzIiLz3OnJcy98pLc2Lzoi4hpymhd9555+L111+/iEPe6qWXXrp49dVXL/Q8fa7EJXHeZbfmpTmxeVGXEJdQWBLnXXZrXpoTmxd1CTldCS3RAomWQctx65L0zFtndV6X0KOhLiEuobAk2ZY6W16aE5sXdQlxCYUlybbU2fLSnNi8qEvIaUtIj/hc65Lo+1uXpGfemFMePS/Nic2LuoS4hMKSZFvqbHlpTmxe1CXkNCWkwb62JLQoRy61PuNaXn0/evS8NCc2L+oS4hIKS5JtqbPlpTmxeVGXkNOUkAb82pLQolxbEj2/dUl65q2zOq9L6NFQlxCXUFiSbEudLS/Nic2LuoQcXkKigdZSkLUl2bIcEpekZ946q/O6hB4NdQlxCYUlybbU2fLSnNi8qEvI6UpIC7C0KFoGOXKpr+W9lrU4c17K3CsvzYnNi7qEuITCkoxY6phTzpyXMvfKS3Ni86IuIacpIVlalnpJblkOiUvSM2+dNUvemLl3XpoTmxd1CXEJhSUZsdTKliVvzNw7L82JzYu6hJyuhEQDvwW9z5q4JM67jN5nTcxLc2Lzoi4hLqGwJM67jN5nTcxLc2Lzoi4hpy2he4lL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hLiEgpL4rz9xbw0JzYv6hJylxKKg3hmzjsWzYnNi7qEuIQC5x2L5mRmup9M6PfYi7qEuIQC5x2L5mRmup9M6PfYi7qE3KWEzAqak5nRGWxF/zWDJfQeW9DvsRd1CXEJ2d3QnMws/u70l/YtqGiW0HusGfVPq9Ql5C4lRL/4WcQLyJQ34/nSnMxs6x1RqdT/dcta/C9iFvXr6XNqo+6IuoS4hMIFZMqb8XxpTma29Y7qAilcQh+jD1iz9QKOEi8gU96M50tzMrPWO1JhqHCiF198EdHPFionfd1SRqPuiLqEpCqhN974wzPoZ7aKF9Aj77e/8cIqel2rOmuv863PdsT50pzMrPWOXELwZI0+YE3rBWwxekl65KXSqdHrWtVZe51vfbYjzpfmZGatd6SiUME8efLkqrUSEv2cS6jhAtbEpfjggw8u9OixLPECbsmrgvnNL7578e6ff/oJ+t4tZVRn7XW+OtuR50tzMrPWO3IJwZM1+oA1rRewRosQF0WP3ktyS16XEIvnS3My2s9+8K0L+t5orXekoiDXSkjq8qH3KOhzZdQdUZeQ05aQFmDp8fbb7148efK1C3qfNfEC9uSty+ef/3jrgkpIfvTyVy72lFGdNdP50pyM5hL6CH2ujLoj6hLiEgoXsCevS2hZPF+ak9FUQhH93Aitd6SioP833FgkLVRG+gtqPU+fK6PuiLqEnK6EWpaj/mPDLcsSL2BP3mwldOT50pyM5hL66Hn6XBl1R9QlxCUULmBPXhWJyqelhFRYX/z8Zy7ofa+ps2Y6X5qT0TKW0JJYUIXKRuXjEgpaL6DmElpWZ810vjQno1EJCf18T6135BKCJ2v0AWtaL0BalqN+aDn02j3LEi9gS16VjwqlpXxEr9Efy7aUUZ010/nSnIxG5SP08z213pFLCJ6s0Qesab0AOcOSbMnrEspfQkKv66H1jpZKSA993VpC+r5LqOECNOD1HwH00POFlkCPa8/rPVuWJV5AS15RCVHJrFEJycgSaj1fneHI86U5GY1Kp0av66H1jlxC8GSNPmBN6wW0Lgktw7Xn9y5JS15xCbmE1rTekYpCxeESAvQBa1ovQAOthwZ/6VEvh+i99LVL6FznS3OyhApjNMqxV+sduYTgyRp9wJrWCzjTkrTkFZeQS2hN6x2pKFQgPUpIz7uEGi5Ag6zBXnrUy6GHvhYty9Ylackre0qoLh8ZWUKt56uzHXm+NCdLqCTuhfJs1XpHLiF4skYfsKb1AlqXpDxcQi6he6E8W7XekagwYgnV1kpoS/nILXe0hLqEHF5Ccm1ZaDm0BPq6fr5lOSRewJa8KiEVCZVOEcumds9/Rb92vnSO8bn4/N7zpTlZQuUwGuXYa+sduYQW0Aes2XoBa0viEvpInbXX+dI5xufi83vPl+ZkCZXEaJRjr613pOKIJXOthFQ24hICWy9ANOCiwW9ZCqH3vSZewJ68KhAVCpVNbU/5SJ211/nWZzvifGlOllBJjEY59tp6Ry6hBfQBa7ZegNQDr0WgRem9JHvyuoSWxfOlORmNiqZGr+th7x2pQGIhXSuhW8pHRt0RdQk5XQnV6gUg9LpW8QJuyatC2YLeZ02dNdP50pyMRqVTo9f1sPeOXEKAPmDN3guo0VLU6HWt4gXckpdKZg29z5o6a6bzpTkZjUpH6Od76nFHsZBa0HusGXVH1CXk9CU0WryATHkzni/NyWhUPkI/31OPO6KiWULvsWbUHVGXEJdQuIBMeTOeL83JaEeUjzz6HVGXEJdQuIBMeTOeL83JaC6hdaPuiLqEuITCBWTKm/F8aU5GcwmtG3VH1CXEJRQuIFPejOdLczLavYsnevQ7oi4hLqFwAZnyZjxfmpPRXELrRt0RdQlxCYULyJQ34/nSnMzs0e+IuoS4hMIFZMqb8XxpTmb26HdEXUJcQuECMuXNeL40JzN79DuiLiEuoXABmfJmPF+ak5k9+h1RlxCXULiATHkzni/Nycwe/Y6oS4hLKFxAprwZz5fmZGaPfkfUJeQuJRR/yTPLlDfj+dKczOzR74i6hLiEgkx5M54vzcnMHv2OqEvIFCX0w+989oK+1yrmHU156Xtn1CsvzcnM4u9OM3dG9HvsRV1C7lJCoz3qUt9Lr7w0JzYv6hKSsoS+9MKnL7739U9d/PInX7jQ98+05MqaJW8xKi/Nic2LuoTcpYTob+RvcW1J9H0tSXzNNfrH0FF5lTVL3mJE3oLmxOZFXUJOX0JxifVcvRxaCj0fXx/R8vRe6mtZs+QtRuQtaE5sXtQlxCU0eKmVKUveYkTegubE5kVdQlKVkJZAw/6nX3/5Ql9rafRz9RLR+49a6jprkSFvzNwzb0FzYvOiLiEuocFLrc8uMuSNmXvmLWhObF7UJeT0JbRlOa59Te8rvZf6WtYseYsReQuaE5sXdQlxCQ1eamXNkrcYkbegObF5UZeQ05eQBj4O/V9/+9ULfV0vTf216I8e8f17L/W1rFnyFiPyFjQnNi/qEuISGrzUypolbzEib0FzYvOiLiGpSkjDryWpF2jLcsiopa6zZskbM/fMW9Cc2LyoS4hLaPBSK2uWvDFzz7wFzYnNi7qEnLaE9BemWoSiXo56KfR1fE2h96Jl6bXUdd46a5Eh76jzLWhObF7UJcQlNHipY44MeUedb0FzYvOiLiGnLSENuAa/iIuytCz1UtBySK+lrrPUWR89b0FzYvOiLiEuoaRLnS1vQXNi86IuIS6hpEudLW9Bc2Lzoi4hpyuhesCXlmPPUtRuXeq1vMqaJW/M3DNvQXNi86IuIS6hwUutrFnyxsw98xY0JzYv6hJymhKqB50GX8tB3yvqpaHPqe1d6jpD/XWdNX5Pzph31PkWNCc2L+oS4hIavNR6Pn5Pzph31PkWNCc2L+oSclgJabA10PrLTy1CHP5rrr1nrf65qHWp9V7Ou/5zkfIWNCc2L+oS4hJKutTZ8hY0JzYv6hJyWAldWw59vXXY92pd6q156T16yJq3oDmxeVGXEJdQ0qXOlregObF5UZeQw0pIf8mp5dDSyOjlkNaldt59lLegObF5UZcQl1DSpc6Wt6A5sXlRl5DDS0jLIEctifOOobwFzYnNi7qEuISSLnW2vAXNic2LuoQcVkLXluBeyyGtS+28+yhvQXNi86IuIS6hpEudLW9Bc2Lzoi4hh5XQWbQu9VlkzVvQnNi8qEuISyjpUmfLW9Cc2LyoS4hLKOlSZ8tb0JzYvKhLiEso6VJny1vQnNi8qEuISyjpUmfLW9Cc2LyoS4hLKOlSZ8tb0JzYvKhLiEso6VJny1vQnNi8qEuISyjpUmfLW9Cc2LyoS8hdSigO4lk573g0JzYv6hLiEvqY845Hc2Lzoi4hLqGPOe94NCc2L+oSMqyEzOyxUZcQl5CZDUFdQlxCZjYEdQlxCZnZENQlxCVkZkNQlxCXkJkNQV1CXEJmNgR1CWkqITOzUVxCZnYol5CZHcolZGaHcgmZ2aFcQmZ2KJeQmR3KJWRmh3IJmdmBXvnwv0a0QE72qF7rAAAAAElFTkSuQmCC)

如果合成表输出有多个物品，则在合成物品处和轮播物品组件一样对输出的物品进行轮播，点击物品能显示物品信息。

  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取预设组件类 TableRecipeComp
        TableRecipeComp = bookManager.GetTableRecipeCompCls()
        # 创建TableRecipeComp实例
        tableRecipeComp = TableRecipeComp()
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
recipeId | str | 合成配方的ID，一般为合成的物品的**identifier** ，支持[自定义配方](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义配方.html>)  
aux | int | 物品的附加值**AuxValue** ，该属性可以不写，默认值为0。  
  * 返回值

数据类型 | 说明  
---|---  
TableRecipeComp | 组件实例  
  * 备注

    * 该函数应该在组件显示之前被调用（也就是在组件调用**Show** 之前）
  * 示例
[code] tableRecipeComp.SetDataBeforeShow("minecraft:honey_bottle", 0)	# 设置蜂蜜的配方
        tableRecipeComp.Show()
```
结果如下图所示：

![tableRecipe2](data:image/png;base64,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)


### 4.EntityComp

#### __init__

  * 描述

实体预览组件初始化

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
EntityComp | 组件实例  
  * 备注

![entity1](/dev/mcmanual/mc-dev/assets/img/entity1.14807163.gif)

该组件在显示的时候，设定的实体会自动进行旋转。

  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取预设组件类 EntityComp
        EntityComp = bookManager.GetEntityCompCls()
        # 创建EntityComp实例
        entityComp = EntityComp()
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
entityName | str | 实体的**identifier** ，比如这里的猫是 "**minecraft:cat** "，支持[自定义生物](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义配方.html>)  
molang_dict | dict | molang表达式字典  
默认值为{}  
entityOffset | tuple(int, int) | 显示的实体相对于外边框的偏移，用于微调，单位为像素。  
默认值为(0, 0)  
backgroundImage | str | 实体显示的背景图片，这里写的是路径字符串，比如要索引”**resource_pack/textures/items/apple.png** “这一图片 就写地址 ”**textures/items/apple** “。  
默认值为**BookConfig.Images.sqrtPanel_light** ，详见[书本配置](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/05-常用脚本对象.html#书本配置>)  
  * 返回值

数据类型 | 说明  
---|---  
EntityComp | 组件实例  
  * 备注

    * 该函数应该在组件显示之前被调用（也就是在组件调用**Show** 之前）
  * 示例
[code] entityName = "minecraft:cow",
        molang_dict = {}
        entityOffset = (0, -20)
        entityComp.SetDataBeforeShow(entityName, molang_dict, entityOffset)	# 设置牛
        entityComp.Show()
```
结果如下图所示：

![entity2](/dev/mcmanual/mc-dev/assets/img/entity2.9619c8ad.gif)


### 5.ProgressBarComp

#### __init__

  * 描述

进度条组件初始化

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
ProgressBarComp | 组件实例  
  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取预设组件类 ProgressBarComp
        ProgressBarComp = bookManager.GetProgressBarCompCls()
        # 创建ProgressBarComp实例
        progressBarComp = ProgressBarComp()
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
value | float | 进度条的值，范围为(0, 1)，**0** 表示进度为**0%** ，**1** 表示进度为**100%** 。  
默认为**1**  
emptyImage | str | 进度条的底图，这里写的是路径字符串，比如要索引”**resource_pack/textures/items/apple.png** “这一图片 就写地址 ”**textures/items/apple** “  
默认值为**BookConfig.Images.progressBar_dark** ，详见[书本配置](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/05-常用脚本对象.html#书本配置>)  
fillImage | str | 进度条的填充图  
默认值为**BookConfig.Images.progressBar_light** ，详见[书本配置](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/05-常用脚本对象.html#书本配置>)  
  * 返回值

数据类型 | 说明  
---|---  
ProgressBarComp | 组件实例  
  * 备注

    * 进度条的显示主要是通过裁剪**fillImage** 从而显示出部分**emptyImage** 。
  * 示例
[code] progressBarComp.SetDataBeforeShow(0.5)	# 设置进度条到50%
        progressBarComp.Show()
```
结果如下图所示：

![progressBar](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAbkAAABDCAYAAAABKnvjAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAANBSURBVHhe7dU9rhxVFEZRj8AhCRIhImcS5IyByfDnwDPCD5gHNuMwTizhwxa3S1XcVxIrWFlL3dI9X+8Xb59+fv9373599bx+e/2/9+fvAFxB5G6oHgqA40TuhuqhADhO5G6oHgqA40TuhuqhADhO5G6oHgqA40TuhuqhADhO5G6oHgqA40TuhuqhADjudOS+/eIlAFyuonWUyAFwSxWto0QOgFuqaB0lcgDcUkXrqMORmz/i1TdfAw/68vOXwINmbypiKyIHG9WQgTZ7UxFbETnYqIYMtNmbitiKyMFGNWSgzd5UxFZEDjaqIQNt9qYitiJysFENGWizNxWxFZGDjWrIQJu9qYitiBxsVEMG2uxNRWxF5GCjGjLQZm8qYisiBxvVkIE2e1MRWxE52KiGDLTZm4rYisjBRjVkoM3eVMRWRA42qiEDbfamIrYicrBRDRloszcVsRWRg41qyECbvamIrYgcbFRDBtrsTUVsReRgoxoy0GZvKmIrIgcb1ZCBNntTEVsROdiohgy02ZuK2IrIwUY1ZKDN3lTEVk5H7ruvPgMeVEMG2uxNRWxF5GCjGjLQZm8qYisiBxvVkIE2e1MRWxE52KiGDLTZm4rYisjBRjVkoM3eVMRWRA42qiEDbfamIrYicrBRDRloszcVsRWRg41qyECbvamIrYgcbFRDBtrsTUVsReRgoxoy0GZvKmIrIgcb1ZCBNntTEVsROdiohgy02ZuK2IrIwUY1ZKDN3lTEVkQONqohA232piK2InKwUQ0ZaLM3FbEVkYONashAm72piK2IHGxUQwba7E1FbEXkYKMaMtBmbypiKyIHG9WQgTZ7UxFbETnYqIYMtNmbitjK4chN80cAwBUqWkeJHAC3VNE6SuQAuKWK1lEiB8AtVbSO+kfkjqrwHfH2zQ+fiu/g+b17+vBe/6E6ToCzRI6HVJiuVMcJcJbI8ZAK05XqOAHOEjkeUmG6Uh0nwFkv3r758UNcLhR/kP/mj1++/0R9hudXYbpSHSfAWSLHQypMV6rjBDhL5HhIhelKdZwAZ4kcD6kwXamOE+Cs6yM3Pf200H+qH4ngPVSYrlTHCXCWyPGQCtOV6jgBznn9/i+5AL5PsWl1XgAAAABJRU5ErkJggg==)


### 6.ButtonComp

#### __init__

  * 描述

按钮组件初始化

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
ButtonComp | 组件实例  
  * 示例
[code] import mod.client.extraClientApi as clientApi
        # 获取书本管理对象，详细用法见“05-常见脚本对象”
        bookManager = clientApi.GetBookManager()
        # 获取预设组件类 ButtonComp
        ButtonComp = bookManager.GetButtonCompCls()
        # 创建ButtonComp实例
        buttonComp = ButtonComp()
```
#### SetDataBeforeShow

  * 描述

在显示组件之前，设置组件的数据

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
defaultImage | str | 按钮正常状态时（没被点击或者鼠标浮动时）显示的图片，这里写的是路径字符串，比如要索引”**resource_pack/textures/items/apple.png** “这一图片 就写地址 **”textures/items/apple** “  
默认值为**BookConfig.Images.blank** ，即空白图片，详见[书本配置](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/05-常用脚本对象.html#书本配置>)  
pressCallBack | dict | 按钮被按下的时候调用的函数，格式见备注。  
默认值为**None**  
moveInCallBack | dict | 按下并滑动到按钮的时候调用的函数，格式见备注。  
默认值为**None**  
text | str | 按钮上的文本  
默认值为""  
pressImage | str | 按钮被点击时显示的图片  
默认值为**None**  
hoverImage | str | 按钮被鼠标浮动时显示的图片  
默认值为**None**  
  * 返回值

数据类型 | 说明  
---|---  
ButtonComp | 组件实例  
  * 备注

    * pressCallBack 和 moveInCallBack 的数据格式是一致的：

参数名 | 数据类型 | 说明  
---|---|---  
func | function | 回调的函数  
args | tuple | 回调函数的参数列表，如果回调函数无参，则该属性无需定义  
  * 示例
[code] def OnPress(arg):
            print "on button press with arg '{0}'".format(arg)
        
        pressCallBackDict = {
            "func": OnPress,
            "args": [1]
        }
        buttonComp.SetDataBeforeShow(defaultImage="textures/items/apple", pressCallBack=pressCallBackDict)
        buttonComp.Show()
```
#### SetAlpha

  * 描述

设置按钮中图片的透明度

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
alpha | float | 透明度值，范围都在(0, 1)内  
  * 返回值

数据类型 | 说明  
---|---  
ButtonComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用控件节点的**SetAlpha** 。


#### SetTextColor

  * 描述

设置按钮中文字的颜色

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
color | tuple(float, float, float, float) | 颜色值，分别为RGBA四个通道值，范围都在(0, 1)内  
  * 返回值

数据类型 | 说明  
---|---  
ButtonComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用**文本控件节点** 的**SetColor** 。


#### SetTextSize

  * 描述

设置按钮中文本字体的大小

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
newSize | int | 要设置的文本字体大小  
  * 返回值

数据类型 | 说明  
---|---  
ButtonComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用**文本控件节点** 的**SetTextFontSize** 。


#### SetTextAlpha

  * 描述

设置按钮中文字的透明度

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
alpha | float | 透明度值，范围都在(0, 1)内  
  * 返回值

数据类型 | 说明  
---|---  
ButtonComp | 组件实例  
  * 备注

    * 在使用该方法之前需保证组件处于显示状态（也就是调用**Show** 之后），因为该方法本质是调用**文本控件节点** 的**SetAlpha** 。


入门

分钟