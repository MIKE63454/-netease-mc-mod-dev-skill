# 页面API

## 1.BasePage

所有书本页面都继承**BasePage** ，页面负责排版页内的组件。

### 1.重写方法

#### __init__

  * 描述

初始化页面

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
size | tuple(int, int) | 页的大小，单位为像素  
默认值为**None** ，书本系统会根据当前书本界面自动获取页面的大小  
position | tuple(int, int) | 页的位置（锚点为左上角），单位为像素  
默认值为**None** ，书本系统会根据当前书本界面自动获取页面的位置  
  * 返回值

数据类型 | 说明  
---|---  
BasePage | 页面实例  
  * 备注

    * 当你自定义一个页类的时候**必须重写类初始化方法** ，并执行父类"**__init__** "方法，如何重写请参考[脚本自定义书本](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义页面>)。


#### Show

  * 描述

显示页面

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
BasePage | 页面实例  
  * 备注

    * 当你自定义一个页面的时候**必须重写该方法** ，并执行父类**Show** 方法，必须返回自身以支持链式调用。如何重写请参考[脚本自定义书本](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义页面>)。
    * 该方法会依次调用页内组件的**Show** 方法，因此在调用该方法前需调用**AddComps** 方法将组件添加至页面中。


#### Hide

  * 描述

隐藏页面

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
BasePage | 页面实例  
  * 备注

    * 当你自定义一个页面的时候**可以不重写该方法** ，但如果重写该方法一定要在最后主动调用父类**Hide** 方法，必须返回自身以支持链式调用。
    * 该方法的调用时机：该方法由书本系统进行调用，当用户翻页的时候，当前页会被隐藏。


### 2.排版方法

BasePage提供了一系列方法方便开发者在页面中进行排版，这些方法依照的UI坐标系参见[“自定义书本UI坐标系”](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/01-自定义基础书本.html#页面编写>)。在对组件调用排版方法之前，确保组件已经加入到页面中（在初始化的时候调用**AddComps** 添加组件），以及调用了页面父类的**Show** 方法。

#### GetPosition

  * 描述

获取页面在书本坐标系中的位置

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 页面在书本坐标系中的位置（锚点在左上角），单位为像素  


#### GetSize

  * 描述

获取页面的大小

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 页面的大小，单位为像素。  


#### Center

  * 描述

获取页面的中心坐标

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
tuple(int, int) | 页面的中心坐标，单位为像素。  


#### Left

  * 描述

获取页面左边界的X值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 页面左边界的X值，单位为像素。  


#### Right

  * 描述

获取页面右边界的X值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 页面右边界的X值，单位为像素。  


#### Top

  * 描述

获取页面上边界的Y值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 页面上边界的Y值，单位为像素。  


#### Bottom

  * 描述

获取页面下边界的Y值

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 页面下边界的Y值，单位为像素。  


#### ResetCompsPosition

  * 描述

重置所有组件的位置为页面当前的位置

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
BasePage | 页面实例  
  * 备注

    * 该函数在调用页面父类的**Show** 方法后使用，在调用组件的排版方法前使用，相当于将组件先对齐页面后再进行平移，具体用法可见[脚本自定义书本](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义页面>)。


### 3.工具方法

#### GetPageGroup

  * 描述

获取页面当前所在的页组对象

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
PageGroup | 页面当前所在的[页组对象](<05-%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC%E5%AF%B9%E8%B1%A1>)  
如果没有则返回**None**  


#### AddComps

  * 描述

向页面中添加组件

  * 参数

参数名 | 数据类型 | 说明  
---|---|---  
comps | 可变长参数，元素为BaseComp | 添加的组件对象  
  * 返回值

数据类型 | 说明  
---|---  
BasePage | 页面实例  
  * 备注

    * 该函数在页面初始化函数中使用，只有将组件添加进页面后，页面在调用**Show** 方法的时候才会调用组件的**Show** 方法，具体用法可见[脚本自定义书本](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/5-自定义书本/02-脚本自定义书本.html#脚本自定义页面>)。


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
            "func": func_1
        }
        callbackDict_2 = {
            "func": func_2,
            "args": [1, 2]
        }
        
        page = BasePage()
        page.Call(callbackDict_1)
        page.Call(callbackDict_2)
```
## 2.TitlePage

### 1.概述

TitlePage提供了对标题的处理方法，方便开发者，具体的示例详见Demo中的“**behavior_pack/customBooks/customBook/entry/myTitlePage.json** ”以及“**behavior_pack/tutorialScripts/myTitlePage.py** ”。

该页面对标题的处理逻辑为：

  1. 当前页面是否为章节的首页，如果是则设置其标题为章节的标题（包括章节的图标）
  2. 当前页面如果不是首页，分两种情况：若传进页面的**Data** 中定义了"**subtitle** "属性，则标题为"**subtitle** "的值，若无，则不显示标题，页面剩余内容自动往上顶。


标题的页面样式可以这样划分，存在大标题的情况下，可以有图标也可以没有图标，不存在大标题但存在小标题的情况下，就显示小标题，我们结合示例中的**json** 文件中的不同属性值来看：

![](data:image/png;base64,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)
```python
    "title": {
        "icon": "textures/items/apple",
        "text": "带图标的标题"
    }
```
如果当前页非首页，**title** 如上定义为一个对象，则显示的效果为带图标的大标题。

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAU8AAABVCAYAAADJ92a7AAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAZZSURBVHhe7d0/bxxFGMdxFAdQJCjSpIIGiYYCiz9FJKiAhgZEhxAVokFIiAZRIDoKhMBOAmlT0FHxAtKBjXkLVEic8zrCjZ3HeTz+7czu7Mzt7vlbfBTfzp/d4u6ru/Od88SDo58fYoH+vrtZ6hpwaRwf7iFCPJdKBa4ldQ24NFQ8LjviuVQqcC2pa8CloeJx2RFPAFnHR3fqESFKefDXbWl1uD8p4gkgS0awlAhkigpnoIK2ScQTQJaMYCkRyBQVzkAFbZOIJ7BYv8yMusZCIpYpq4O9jSOewGKpgE1JXWMhEcgUFbfWiCewWCpgU1LXWEgEMkXFrTXiCWAxjtehzFkd/HTm+HC/GeIJYDFULGPEEwAiKpYx4gkACSqcKep9yzGIJ4BFUoFMUQEcg3gCWCQVyBQVwDGIJ4BFUoGsSQXTI54AFkkFryYVTI94AlgkFbyaVDA94glg+daxU859vCjcFlaHt84cO/746uCx//7cO0E8ASzfOoIK8QSAlHUEFeIJACnrCCrEEwBS1hFUiCcApKwjqBBPAEhyf1d0HcSW8Vwd7J8gngC2APEEgALEEzPw+jPXHu5euSLHagh7t9wfl5GP5x3ncUhrsbgST1zQOm7EE/URT8zAlPG0sSHUPt5bz9+Q62JhrjqeE58PUyCemIHWUUjtf/Tbt2fjfag9Yrs7O3Jt7Gy+GOty9+sPz50LUyGemAELgxqrofX+MYvnNx+/o8d7XM+mrxlDEU/MQOtQbDpEQ595evaSvyu8mAviiRlIxWSsN29cb7q/MiaeqTHMCfHEDNQKRng/0PbKUetr8fG8MNZxDT7yXfx8TI14YgZqBeLm9WfP7ZUSflGk9qihJJ7+eBc/H1MjnmhAPfBbUef3uub64yVWh+sHitvPK3nZbsfUvmo+pkY80YA92FsLzzTV+T0/3x//4OUXzo0N5fdS1Bqva358PDeGqRBPzIDF4Z/7P8jxUravUXMUm//77c/leAvxNfrb8RjmgHhiBlrFwfY1H918Sc6L2Xw1lhI+ZjSUrY3P6W/HY5gD4okZaBUH29f7/rP35FzP5qqxLv4cQ1jQ7Xa8nxrDHBBPTMzCEJ6FhX8/fftVOW+I8PI/7BX/9j2Eyn62uep88Zy+X4kM+8dsLxOP21obV7fjMcwB8cSE7LONFrBakYj38bdTY13H1Jwh+qyP5/jb8RjmgHhiQkuJZ/Dl+2+cHEt9RKmL2i8Wz/G34zHMAfHEhHavXu0MxhjxPv62vT3w7x/7cm7XsdTxnD7r/Bx728Fu+58xF8QTE0kFoTQW9hfp42eHar/UObrGUmtifp6tU/waPzf8fPae7c7OhXmYGvHERLrikRtL+fW7T+Q6tV/qHF1jqTWefcPo/r2vTm8/Wqf4dfaM87VrT58dU/MwB8QTE0lFoXYw1H6pc6TGwp+KC2Opjzx1xTOe56Xm9FmPTSOemIDFRY0F9tXJ8K8aHyqOj32EyH9UyIvneyGIqfFgaDzDL8zCuP/QfOxkD16+zwjxxIblQuINmZvi9+mzZ2pO7Xjm9vKGzEVrxBMbdPLgH/DsKXy3vEYw4j0sal1y58w+QwwefZLAU/MD/l+iJSKe2JBcQLqE35yfrF3HSI33MfTcpdca+LX2sz+GbUE8sQEhHO+++Jwc62tMgPqs9eM1zzXkDzT35c+HqRBPNFbrAW/f8Cn5M3F9rsHP6TNfCb+BD+vil+G2Xy1+b0yFeKIB/0BPvT9Ywu8dqDmxvnNfeerJ07kFbxEMuR5sA+KJBiwkLWJiX68csv+Q6yi95rCu1kersATEEwAKEE8AKEA8AaAA8QSAAsQTAAoQTwAoQDwBoADxBIACxBMAChBPACiwwHje+/EL4AJ1X8kZu0e8HjAqgqWIJ5pS95WcsXvE6wGjIliKeKIpdV/JGbtHvB4wKoKlBsVTXQwQU/edHLWPotbmqH2AmApkCvFEdeq+k6P2UdTaHLUPEFOBTCGeqE7dd3LUPopam6P2AWIqkCmD4pmiLgZQ95WcsXvE6wGjIliKeKIpdV/JGbtHvB4wKoKliCeaUveVnLF7xOsBoyJYqlo8AWB6/htGOnq1EE8A26PxVzI94glgexBPAChAPAGgAPEEgALEEwAKEE8AKCAi1wrxBLA9RORaIZ4AtoeIXCun8bz18H/jNndC4GHxMgAAAABJRU5ErkJggg==)
```python
    "title": "大标题"
```
如果当前页非首页，**title** 如上定义为一个字符串，则显示的效果为不带图标的大标题。

![](data:image/png;base64,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)
```python
    "subtitle": "默认的副标题"
```
如果当前页非首页，只定义**subtitle** 而不定义**title** 则显示如上的文本副标题。

### 2.方法

#### SetTitleData

  * 描述

设置标题的数据

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
TitlePage | 页面实例  
  * 备注

    * 该函数是在你自定义页面重写**Show** 方法的时候调用，往标题中注入数据，如何使用见Demo。


#### LayoutTitle

  * 描述

排版页面的标题

  * 参数

无

  * 返回值

数据类型 | 说明  
---|---  
int | 排版标题后，标题的下边界，单位为像素  
  * 备注

    * 该函数是在你自定义页面重写**Show** 方法的时候调用，用于排版标题，如何使用见Demo。


入门

分钟