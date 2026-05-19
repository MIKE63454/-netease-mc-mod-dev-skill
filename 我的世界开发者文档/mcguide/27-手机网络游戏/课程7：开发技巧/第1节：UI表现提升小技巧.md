# UI界面表现提升技巧

## 实现UI管理器对象

  * UI界面要最终显示，需要经历【注册】，【创建】，【显示】这三个关键步骤，并且，这三个关键步骤，是可以分离的。


### UI界面的【注册】，

  * 通过API【RegisterUI】注册一个UI界面，在没有卸载Mod的情况下，同一UI只需要注册一次
  * 经测试，注册界面消耗的时间，与界面本身的复杂度（控件数量、用到的资源图片）基本无关，是一个相对稳定的均值。


### UI界面的【创建】

  * 通过API【CreateUI】创建一个UI界面，在没有卸载Mod，并且没有调用API【SetRemove】销毁界面的情况下，同一个UI只需要创建一次
  * 创建UI界面的整个流程中，在生成了UI上的全部控件之后，会调用UI界面对应的类的【Create】成员函数，一般会在这个成员函数中对按钮的响应等功能进行初始化，假如【Create】成员函数中执行了【Clone】等消耗较大的API， 那么也会拖慢整个UI的创建流程
  * 经测试，创建界面消耗的时间，受界面本身的复杂度影响，一个UI界面包含的控件数量越多，创建的速度就越慢，一个UI界面使用到的图片资源的总尺寸越大，创建的速度就越慢
  * 一般来说，单个不是特别复杂的UI界面的创建时间，在10-40ms之间，按照游戏帧率30来计算，每帧的平均时间为33ms，40ms左右的时间刚刚好不至于产生能够感知到的卡顿。


### UI界面的【显示】

  * 通过API【SetScreenVisible】显示/隐藏一个UI界面。
  * 在UI界面已经完成创建的情况下，仅仅使用API【SetScreenVisible】显示一个UI界面，基本都仅仅消耗1ms左右。
  * 经测试，对UI里面的控件的具体显示功能进行调整，那么消耗的时间回合具体调整的内容相关。
  * 对图片控件进行【SetVisible】、【SetSprite】等操作，消耗的时间与对应的图片资源的尺寸相关
  * 对文本控件进行【SetText】、【SetTextColor】等操作，消耗的时间与文本的总长度相关，并且设置中文比设置纯英文和数字慢得多。
  * 对控件进行【Clone】操作，与控件本身的复杂度（子控件的数量）正相关。


### UI界面的【销毁】

  * 通过API【SetRemove】销毁一个UI界面，调用【SetRemove】之后，需要重新【CreateUI】后才能显示这个UI。
  * 经测试，【SetRemove】消耗的时间在1-3ms左右，基本不太可能导致可感知的客户端卡顿。


### 管理器代码示例

  * 使用管理器分离UI界面的【注册】、【创建】和【显示】，可以有效避免**同时大量创建界面** 的情况发生，避免客户端出现可感知的卡顿。
  * 管理器基于**Create On Use** 的原则设计，通过管理器获取/显示UI界面，可以基本保证**同一时间仅创建一个UI** 与**每个UI仅创建一次**
  * 管理器提供**CleanCacheUI** 函数，可以在合适的时候调用释放已经完成【创建】的UI界面，以释放内存占用。
```python
    class UIDef:
    	UIBig1 = "UIBig1"
    	UIBig2 = "UIBig2"
    	...
    
    UIData = {
    	UIDef.UIBig1 : {
    		"cls":"neteaseUiSampleScript.ui.netease_uisample_big.UiSampleScreen",
    		"screen":"netease_uisample_big01.main",
    		"isHud":1,
    		"layer":clientApi.GetMinecraftEnum().UiBaseLayer.PopUpLv1,
    	},
    	UIDef.UIBig2 : {
    		"cls":"neteaseUiSampleScript.ui.netease_uisample_big.UiSampleScreen",
    		"screen":"netease_uisample_big03.main",
    		"isHud":1,
    		"layer":clientApi.GetMinecraftEnum().UiBaseLayer.PopUpLv1,
    	},
    	UIDef.UIBig3 : {
    		...
    }
    
    class UIMgr(object):
    	def __init__(self):
    		super(UIMgr, self).__init__()
    		self.mModName = "NeteaseUiSample"
    		self.mUIDict = {}
    		self.mClientSystem = None
    
    	def Destroy(self):
    		pass
    	# UI管理器的初始化，一般在客户端引擎事件【UiInitFinished】的回调中调用
    	def Init(self, system):
    		self.mClientSystem = system
    		# 在初始化时，执行所有可能用到的UI的【注册】环节
    		# UI的【注册】环节消耗比较大，在初始化时执行主要是因为客户端初始化的整个流程一般会消耗好几秒，额外消耗几十到一百多毫秒用来【注册】UI界面，影响不是很大。
    		for uiKey, config in UIData.iteritems():
    			cls, screen = config["cls"], config["screen"]
    			clientApi.RegisterUI(self.mModName, uiKey, cls, screen)
    
    	# 封装了UI的【创建】过程
    	def CreateSingleUI(self, uiKey):
    		config = UIData.get(uiKey, None)
    		if not config:
    			return None
    		extraParam = {}
    		if config.has_key("isHud"):
    			extraParam["isHud"] = config["isHud"]
    		ui = clientApi.CreateUI(self.mModName, uiKey, extraParam)
    		if not ui:
    			return None
    		layer = config.get("layer", None)
    		if not layer is None:
    			ui.InitLayer(layer)
    		ui.InitSystem(self.mClientSystem, uiKey)
    		ui.InitScreen()
    		# 缓存已经完成【创建】的UI
    		self.mUIDict[uiKey] = ui
    		return ui
    
    	def GetUI(self, uiKey):
    		# 优先从缓存中获取UI界面对象
    		ui = self.mUIDict.get(uiKey, None)
    		if ui:
    			return ui
    		# 假如没有缓存，那么新【创建】一个
    		return self.CreateSingleUI(uiKey)
    
    	# 【显示】一个UI界面
    	# 假如UI还没有【创建】过，那么先执行【创建】流程
    	# 假如UI已经完成了【创建】，那么直接从缓存中获取UI实例
    	# 在一般的应用中，只有第一次【显示】UI界面的时候，才会附带一个额外的【创建】流程
    	# 之后的再次【显示】界面，就不需要再次【创建】界面，大大提升界面【显示】的速度
    	def ShowUI(self, uiKey):
    		ui = self.GetUI(uiKey)
    		if not ui:
    			return
    		ui.Show()
    
    	def CleanCacheUI(self):
    		if not self.mUIDict:
    			return
    		for uiKey, ui in self.mUIDict.iteritems():
    			ui.SetRemove()
    		self.mUIDict.clear()
    
    	def RemoveUI(self, uiKey):
    		ui = self.mUIDict.get(uiKey, None)
    		if not ui:
    			return False
    		del self.mUIDict[uiKey]
    		ui.SetRemove()
    		return True
```
## 使用pushScreen优化单个UI的创建

  * API【pushScreen】整合了UI界面的【创建】和【显示】两个环节，只需要【注册】和【pushScreen】即可显示一个UI
  * 对于同一个UI界面，【pushScreen】不建议与【CreateUI】混用
  * 【pushScreen】只能同时显示一个UI界面，在一个UI界面显示的状态下，调用【pushScreen】显示另外一个UI界面，会自动隐藏之前的UI界面
  * 使用【pushScreen】【创建】并【显示】一个UI界面，实际上是一个异步的过程，所以相对于【CreateUI】方法来说，整个界面的【创建】工作是分时完成的，可以最大限度地防止客户端卡顿地现象发生。
  * 【CreateUI】的执行流程 ![](data:image/png;base64,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)
  * 【pushScreen】的执行流程 ![](/dev/mcmanual/mc-dev/assets/img/help_ui_02.2941b3a9.png)
  * 【pushScreen】【创建】界面是一个异步的过程，从流程图上可见，必须等待引擎调用注册时输入的ScreenNode类的【Create】成员函数后，界面的初始化工作才完成，之后才能正常地对界面上地控件进行各种调整。


## 其他小技巧

### 使用九宫贴图替代大尺寸贴图

  * 当前UI编辑器中，已经提供了非常好用的九宫拉伸预览功能，使用九宫拉伸，可以用很小尺寸的贴图，实现各种背景贴图效果 ![](/dev/mcmanual/mc-dev/assets/img/help_ui_03.cec21fb2.png) 使用一张32X64的小尺寸贴图，通过九切拉伸，可以展现出297X198的背景图效果，如下图 ![](/dev/mcmanual/mc-dev/assets/img/help_ui_04.8f59f5ac.png)


### 拆分含有分页的复杂界面

  * 经测试，单个UI界面【创建】和【显示】需要消耗的时间，受整个UI界面中的控件总数影响最大，控件总数越多，【创建】和【显示】消耗的时间越大。
  * 那么当一个界面上由于功能太多，导致控件太多，最终导致【创建】【显示】消耗时间太多导致客户端出现卡顿时，就可以尝试把一个界面拆分为多个子界面的方式来优化客户端表现。
  * 以领地插件为例，下面是领地插件界面的示意图 ![](/dev/mcmanual/mc-dev/assets/img/help_ui_05.61a3cf37.png)
  * 可见界面整体是非常复杂的，每个分页上面就含有很多控件，并且整个界面还有五个分页 ![](data:image/png;base64,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)
  * 为了优化整个界面的实际表现，从UI的工程上就可以看到，事实上每个分页都是一个独立的子界面，表面上切换分页的行为，最终在代码上实际是用隐藏一个界面然后显示另外一个界面的方式来实现的。 示例代码如下：
```python
    def OnApplyBtn(self, args):
    	"""
    	切换领地界面到【申请权限】分页
    	"""
    	touchEvent = args["TouchEvent"]
    	touch_event_enum = extraClientApi.GetMinecraftEnum().TouchEvent
    	if touchEvent == touch_event_enum.TouchUp:
    		for keys in uiDef.UIData.keys():
    			if keys != uiDef.UIDef.UIResidenceApply:
    				ui = self.mUIMgr.GetUI(keys)
    				ui.ClosePanel()
    		ui = self.mUIMgr.GetUI(uiDef.UIDef.UIResidenceApply)
    		if ui:
    			ui.Show()
```
### 分时执行Clone等高消耗的操作

  * 经测试，【创建】并【显示】下面的背景图+一个组合控件+一个ScrollView+一些按钮消耗的时间，仅为Clone100个组合控件的1/25（在一般配置的PC环境，【创建】并【显示】消耗36ms；而Clone100个组合控件消耗867ms） ![](/dev/mcmanual/mc-dev/assets/img/help_ui_07.8e87a5a7.png)
  * 那么当最终想要下图所示的含有100个组合控件的界面时，最好是最初时候仅【创建】并【显示】不含有组合控件的简单界面（初始化成员函数【Create】中隐藏组合控件的原型）。 ![](/dev/mcmanual/mc-dev/assets/img/help_ui_08.68434471.png)
  * 然后在接下来的几十帧中，每帧创建个5-10个组合控件，直到全部创建完成 示例代码如下：
```python
    def PlusSomePart(self, plusNum):
    	baseOffset = len(self.mOnUseParts)
    	basePos, baseSize = self.mScrollData["partBasePos"], self.mScrollData["partBaseSize"]
    	offset = self.mScrollData["partBaseOffset"]
    	for i in xrange(plusNum):
    		data = self.GetFreePart()
    		pos = (basePos[0], basePos[1]+(baseSize[1]+offset)*(baseOffset+i))
    		baseUIControl = self.GetBaseUIControl(data["fullName"])
    		baseUIControl.SetPosition(pos)
    		baseUIControl.SetVisible(True)
    		self.mOnUseParts.append(data["id"])
    	self.ResizeScroll()
    
    def OnButtonAsyncPlus(self, plusNum, args):
    	self.mAyncPlusLeftNum += plusNum
    
    def Update(self):
    	if self.mAyncPlusLeftNum <= 0:
    		return
    	plusNum = min(5, self.mAyncPlusLeftNum)
    	self.mAyncPlusLeftNum -= plusNum
    	self.PlusSomePart(plusNum)
```
### 主动使用过渡性的加载进度界面

  * UI界面，在客户端Mod没有卸载的情况下，仅需要【注册】、【创建】一次，之后就可以多次重复【显示/隐藏】。
  * 经测试，对比【注册】【创建】行为，【显示/隐藏】消耗的时间基本都是10ms以内，哪怕此界面控件繁多，逻辑复杂。
  * 可以在游戏启动后，主动添加一个带进度条的全屏界面，使用每帧【注册】并【创建】一个UI界面的方法，把需要用到的界面全部创建好，那么在游戏过程中，仅调用【显示/隐藏】功能的界面就不会卡顿了。


https://mc.res.netease.com/pc/zt/20201109161633/mc-dev/assets/img/help_ui_04.8f59f5ac.png

进阶

20分钟