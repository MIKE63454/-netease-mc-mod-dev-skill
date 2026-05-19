# 玩家对象PlayerObject

## 概述

  * 继承关系


  * 描述

PlayerObject（玩家对象）是对玩家对象封装的基类，它为实体提供了面向对象的使用方式。

  * 成员变量

无


## 索引

接口 |  | 描述  
---|---|---  
[GetPlayerId](<#getplayerid>) | **[客户端]**/**[服务端]** | 获取玩家预设的玩家ID  
[IsLocalPlayer](<#islocalplayer>) | **[客户端]**/**[服务端]** | 判断当前玩家对象是否本地玩家  
[IsSneaking](<#issneaking>) | **[服务端]** | 是否潜行  
[GetHunger](<#gethunger>) | **[服务端]** | 获取玩家饥饿度，展示在UI饥饿度进度条上，初始值为20，即每一个鸡腿代表2个饥饿度。 **饱和度(saturation)** ：玩家当前饱和度，初始值为5，最大值始终为玩家当前饥饿度(hunger)，该值直接影响玩家**饥饿度(hunger)** 。  
1）增加方法：吃食物。  
2）减少方法：每触发一次**消耗事件** ，该值减少1，如果该值不大于0，直接把玩家 **饥饿度(hunger)** 减少1。  
[SetHunger](<#sethunger>) | **[服务端]** | 设置玩家饥饿度。  
[SetStepHeight](<#setstepheight>) | **[服务端]** | 设置玩家前进非跳跃状态下能上的最大台阶高度, 默认值为0.5625，1的话表示能上一个台阶  
[GetStepHeight](<#getstepheight>) | **[服务端]** | 返回玩家前进非跳跃状态下能上的最大台阶高度  
[ResetStepHeight](<#resetstepheight>) | **[服务端]** | 恢复引擎默认玩家前进非跳跃状态下能上的最大台阶高度  
[GetExp](<#getexp>) | **[服务端]** | 获取玩家当前等级下的经验值  
[AddExp](<#addexp>) | **[服务端]** | 增加玩家经验值  
[GetTotalExp](<#gettotalexp>) | **[服务端]** | 获取玩家的总经验值  
[SetTotalExp](<#settotalexp>) | **[服务端]** | 设置玩家的总经验值  
[IsFlying](<#isflying>) | **[服务端]** | 获取玩家是否在飞行  
[ChangeFlyState](<#changeflystate>) | **[服务端]** | 给予/取消飞行能力，并且进入飞行/非飞行状态  
[GetLevel](<#getlevel>) | **[服务端]** | 获取玩家等级  
[AddLevel](<#addlevel>) | **[服务端]** | 修改玩家等级  
[SetPrefixAndSuffixName](<#setprefixandsuffixname>) | **[服务端]** | 设置玩家前缀和后缀名字  
[EnableKeepInventory](<#enablekeepinventory>) | **[服务端]** | 设置玩家死亡不掉落物品  
[AddAnimation](<#addanimation>) | **[客户端]** | 增加玩家渲染动画  
[SetHealthLevel](<#sethealthlevel>) | **[服务端]** | 设置玩家健康临界值，当饥饿值大于等于健康临界值时会自动恢复血量，开启饥饿值且开启自然恢复时有效.原版默认值为18  
[SetStarveLevel](<#setstarvelevel>) | **[服务端]** | 设置玩家饥饿临界值，当饥饿值小于饥饿临界值时会自动扣除血量，开启饥饿值且开启饥饿掉血时有效。原版默认值为1  
[SetNaturalStarve](<#setnaturalstarve>) | **[服务端]** | 设置是否开启玩家饥饿掉血，当饥饿值小于饥饿临界值时会自动扣除血量，开启饥饿值且开启饥饿掉血时有效.原版默认开启  
[SetStarveTick](<#setstarvetick>) | **[服务端]** | 设置玩家饥饿掉血速度，当饥饿值小于饥饿临界值时会自动扣除血量，开启饥饿值且开启饥饿掉血时有效  
[SetNaturalRegen](<#setnaturalregen>) | **[服务端]** | 设置是否开启玩家自然恢复  
[SetHealthTick](<#sethealthtick>) | **[服务端]** | 设置玩家自然恢复速度  
[SetMaxExhaustionValue](<#setmaxexhaustionvalue>) | **[服务端]** | 设置玩家最大消耗度(maxExhaustion)  
[SetPickUpArea](<#setpickuparea>) | **[服务端]** | 设置玩家的拾取物品范围  
[SetJumpable](<#setjumpable>) | **[服务端]** | 设置玩家是否可跳跃  
[SetMovable](<#setmovable>) | **[服务端]** | 设置玩家是否可移动  
[AddAnimationController](<#addanimationcontroller>) | **[客户端]** | 增加玩家渲染动画控制器  
[AddAnimationIntoState](<#addanimationintostate>) | **[客户端]** | 在玩家的动画控制器中的状态添加动画  
[AddGeometry](<#addgeometry>) | **[客户端]** | 增加玩家渲染几何体  
[AddParticleEffect](<#addparticleeffect>) | **[客户端]** | 增加玩家特效资源  
[AddRenderController](<#addrendercontroller>) | **[客户端]** | 增加玩家渲染控制器  
[AddRenderMaterial](<#addrendermaterial>) | **[客户端]** | 增加玩家渲染需要的材质  
[AddSoundEffect](<#addsoundeffect>) | **[客户端]** | 增加玩家音效资源  
[AddTexture](<#addtexture>) | **[客户端]** | 增加玩家渲染贴图  
[SetSkin](<#setskin>) | **[客户端]** | 更换原版自定义皮肤  
  
## GetPlayerId

**[客户端]**/**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

获取玩家预设的玩家ID

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
str | 玩家ID  


## IsLocalPlayer

**[客户端]**/**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

判断当前玩家对象是否本地玩家

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | 是本地玩家则返回True，否则返回False，服务端也返回False  


## IsSneaking

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

是否潜行

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | 是否潜行  
  * 示例
```python
    self.IsSneaking()
```
## GetHunger

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

获取玩家饥饿度，展示在UI饥饿度进度条上，初始值为20，即每一个鸡腿代表2个饥饿度。 **饱和度(saturation)** ：玩家当前饱和度，初始值为5，最大值始终为玩家当前饥饿度(hunger)，该值直接影响玩家**饥饿度(hunger)** 。  
1）增加方法：吃食物。  
2）减少方法：每触发一次**消耗事件** ，该值减少1，如果该值不大于0，直接把玩家 **饥饿度(hunger)** 减少1。

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
float | 玩家饥饿度  
  * 示例
```python
    self.GetPlayerHunger(playerId)
```
## SetHunger

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家饥饿度。

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
value | float | 饥饿度  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetPlayerHunger(playerId, 10)
```
## SetStepHeight

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家前进非跳跃状态下能上的最大台阶高度, 默认值为0.5625，1的话表示能上一个台阶

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
stepHeight | float | 最大高度，需要大于0  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 备注

    * 为了避免因浮点数误差导致错误，设置的时候通常会增加1/16个方块大小，即0.0625。所以此处我们设置2.0625。游戏中默认值是0.5625，即半格高度。
    * 只对玩家生效，无法修改其它实体该属性
    * 修改后不影响跳跃逻辑及跳跃高度，并不会因此而跳到更高，因此在某些特定情况下，你可以走上方块但跳不上去。
  * 示例
```python
    #如果前面放置有两格高的方块，玩家按前进能直接上去，无须跳跃
    self.SetPlayerStepHeight(playerId, 2.0625)
```
## GetStepHeight

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

返回玩家前进非跳跃状态下能上的最大台阶高度

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
float | 台阶高度  


## ResetStepHeight

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

恢复引擎默认玩家前进非跳跃状态下能上的最大台阶高度

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  


## GetExp

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

获取玩家当前等级下的经验值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
isPercent | bool | 是否为百分比  
  * 返回值

数据类型 | 说明  
---|---  
float | 玩家经验值  
  * 备注

    * 如果设置返回百分比为False，则返回玩家当前等级下经验的绝对值（非当前玩家总经验值）。
  * 示例
```python
    print(self.GetExp(False))
```
## AddExp

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家经验值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
exp | int | 玩家经验值，可设置负数  
  * 返回值

数据类型 | 说明  
---|---  
bool | 设置是否成功  
  * 备注

    * 如果设置的exp值为负数，且超过当前等级已有的经验值，调用接口后，该玩家等级不会下降但是经验值会置为最小值
  * 示例
```python
    self.AddExp(25)
```
## GetTotalExp

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

获取玩家的总经验值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 总经验值，正整数。获取失败的情况下返回-1。  
  * 示例
```python
    print(self.GetTotalExp())
```
## SetTotalExp

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家的总经验值

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
exp | int | 总经验值，正整数  
  * 返回值

数据类型 | 说明  
---|---  
bool | 设置是否成功  
  * 备注

    * 根据总经验值会重新计算等级，该接口可引起等级的变化
    * 内部运算采用浮点数，数值较大时会出现误差
  * 示例
```python
    self.SetTotalExp(25)
```
## IsFlying

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

获取玩家是否在飞行

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
bool | True:是 False:否  


## ChangeFlyState

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

给予/取消飞行能力，并且进入飞行/非飞行状态

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
isFly | bool | 飞行状态，True：飞行模式，False：正常行走模式  
  * 返回值

数据类型 | 说明  
---|---  
bool | True:是 False:否  
  * 示例
```python
    self.ChangeFlyState(True)
```
## GetLevel

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

获取玩家等级

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 玩家等级  


## AddLevel

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

修改玩家等级

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
level | int | 玩家等级，可设置负数  
  * 返回值

数据类型 | 说明  
---|---  
bool | 设置是否成功  
  * 示例
```python
    self.AddLevel(2)
```
## SetPrefixAndSuffixName

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家前缀和后缀名字

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
prefix | str | 前缀内容  
prefixColor | str | 前缀内容颜色描述，如 'RED'  
suffix | str | 后缀内容  
suffixColor | str | 后缀内容颜色描述，如 'RED'  
  * 返回值

数据类型 | 说明  
---|---  
bool | 设置是否成功  
  * 示例
```python
    self.SetPrefixAndSuffixName(playerId, "红队", 'RED', '肉盾', 'RED')
```
## EnableKeepInventory

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家死亡不掉落物品

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
enable | bool | 是否开启“保留物品栏”  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.EnableKeepInventory(True)
```
## AddAnimation

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家渲染动画

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
animationKey | str | 动画键  
animationName | str | 动画名称  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.AddAnimation("move.arms", "animation.player.move.arms_custom")
```
## SetHealthLevel

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家健康临界值，当饥饿值大于等于健康临界值时会自动恢复血量，开启饥饿值且开启自然恢复时有效.原版默认值为18

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
healthLevel | int | 健康临界值  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetHealthLevel(16)
```
## SetStarveLevel

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家饥饿临界值，当饥饿值小于饥饿临界值时会自动扣除血量，开启饥饿值且开启饥饿掉血时有效。原版默认值为1

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
starveLevel | int | 饥饿临界值  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetStarveLevel(2)# 饥饿值小于等于2就会进入饥饿掉血状态，默认每隔4秒掉1点血量
```
## SetNaturalStarve

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置是否开启玩家饥饿掉血，当饥饿值小于饥饿临界值时会自动扣除血量，开启饥饿值且开启饥饿掉血时有效.原版默认开启

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
value | bool | True开启，False关闭  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetNaturalStarve(False)# # 关闭饥饿掉血，即使饥饿值小于饥饿临界值时也不会扣除血量
```
## SetStarveTick

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家饥饿掉血速度，当饥饿值小于饥饿临界值时会自动扣除血量，开启饥饿值且开启饥饿掉血时有效

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
starveTick | int | 饥饿掉血速度  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetStarveTick(40) # 饥饿掉血状态下每隔2（40/20）秒扣除1点血量
```
## SetNaturalRegen

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置是否开启玩家自然恢复

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
value | bool | True开启，False关闭  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetNaturalRegen(False) # 关闭自然恢复，即使饥饿值大于健康临界值时也不会恢复血量
```
## SetHealthTick

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家自然恢复速度

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
healthTick | int | 自然恢复速度  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetHealthTick(40) # 自然恢复状态下每隔2（40/20）秒恢复1点血量
```
## SetMaxExhaustionValue

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家最大消耗度(maxExhaustion)

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
value | float | 最大消耗度(maxExhaustion)  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetMaxExhaustionValue(10.0)
```
## SetPickUpArea

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家的拾取物品范围

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
area | tuple(float,float,float) | 拾取物品范围，传入(0, 0, 0)时视作取消设置  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetPickUpArea((5, 0, 3))
```
## SetJumpable

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家是否可跳跃

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
isJumpable | bool | 是否可跳跃,True允许跳跃，False禁止跳跃  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetJumpable(False)
```
## SetMovable

**[服务端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

设置玩家是否可移动

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
isMovable | bool | 是否可移动,True允许移动，False禁止移动  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetMovable(False)
```
## AddAnimationController

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家渲染动画控制器

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
animationControllerKey | str | 动画控制器键  
animationControllerName | str | 动画控制器名称  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.AddAnimationController("root", "controller.animation.player.root_custom")
```
## AddAnimationIntoState

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

在玩家的动画控制器中的状态添加动画

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
animationControllerName | str | 动画控制器名称  
stateName | str | 动画控制器名称  
animationName | str | 添加的动画名称或动画控制器名称  
condition | str | 动画控制表达式  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 示例
```python
    self.AddAnimationIntoState("root", "first_person", "first_person_attack_controller_new", "query.mod.index > 0")
```
## AddGeometry

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家渲染几何体

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
geometryKey | str | 渲染几何体键  
geometryName | str | 渲染几何体名称  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 示例
```python
    self.AddGeometry("default", "geometry.player.custom")
```
## AddParticleEffect

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家特效资源

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
effectKey | str | 特效资源Key  
effectName | str | 特效资源名称  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 示例
```python
    self.AddParticleEffect("nectar_dripping", "minecraft:nectar_drip_particle")
```
## AddRenderController

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家渲染控制器

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
renderControllerName | str | 渲染控制器名称  
condition | str | 渲染控制器条件  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 示例
```python
    self.AddRenderController('custom_render_controller_name', 'query.mod.condition')
```
## AddRenderMaterial

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家渲染需要的材质

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
materialKey | str | 材质key  
materialName | str | 材质名称  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 示例
```python
    self.AddRenderMaterial('custom_material_key', 'custom_material_name')
```
## AddSoundEffect

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家音效资源

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
soundKey | str | 音效资源Key  
soundName | str | 音效资源名称  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 示例
```python
    self.AddSoundEffect("sound_thunder", "ambient.weather.thunder")
```
## AddTexture

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

增加玩家渲染贴图

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
geometryKey | str | 贴图键  
geometryName | str | 贴图路径  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否成功  
  * 备注

    * 调用该接口后需要调用RebuildPlayerRender才会生效
  * 示例
```python
    self.AddTexture("default", "textures/misc/missing_texture")
```
## SetSkin

**[客户端]**

method in Preset.Model.Player.PlayerObject.PlayerObject

  * 描述

更换原版自定义皮肤

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
skin | str | 贴图路径，以textures\models为当前路径的相对路径  
  * 返回值

数据类型 | 说明  
---|---  
bool | 是否设置成功  
  * 示例
```python
    self.SetSkin("kagura")
```