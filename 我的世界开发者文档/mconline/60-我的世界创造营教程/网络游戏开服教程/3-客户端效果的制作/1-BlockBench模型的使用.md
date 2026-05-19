# BlockBench模型的使用

在制作游戏玩法时，经常会遇到需要修改玩家模型，或者实体模型的场景。我们可以使用Blockbench制作的模型，配合模组SDK来轻松实现修改实体模型的功能。

## 本地玩家模型的修改

在开始之前，首先前往内容库，找到`Spigot示例Demo`，下载并打开。本节课将使用官方Demo中提供的模型素材来进行演示。

接下来创建一个插件，这里命名为`testModel`，复制到Mod部署目录。

打开Demo的目录，找到`SpigotDemo\CustomHumanModelDemo\DemoClientMod\resource_packs\resource_pack_geyser_demo_mod`，为Demo中的资源包文件夹，将其中的`models`和`textures`复制到`testModel`的资源包文件夹中。

![](/dev/mcmanual/mc-dev/assets/img/10.b6413047.png)

通过IDE打开这个目录，可以看到目录结构中，主要是复制了2个`.geo.json`后缀的模型文件和1个贴图。

接下来我们尝试，将玩家的模型替换为`player.geo.json`所存储的模型，贴图替换为`player.png`的贴图。

![](/dev/mcmanual/mc-dev/assets/img/11.605ea501.png)

首先要替换模型，我们要知道模型的identifier，打开`player.geo.json`，可以看到identifier字段的值为`geometry.player`，这个值就是模型的识别符，需要保证它在所有资源包中是唯一的。

![](/dev/mcmanual/mc-dev/assets/img/12.1176539a.png)

接下来我们还需要知道模型所对应的贴图的路径。在资源包中对应路径`textures/entity/player`

![](data:image/png;base64,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)

接下来我们就可以调用在[OnLocalPlayerStopLoading (opens new window)](<https://mc.163.com/dev/mcmanual/mc-dev/mcdocs/1-ModAPI/%E4%BA%8B%E4%BB%B6/%E4%B8%96%E7%95%8C.html?key=OnLocalPlayerStopLoading&docindex=3&type=0>)事件触发时，为本地玩家[重建数据渲染器 (opens new window)](<https://mc.163.com/dev/mcmanual/mc-dev/mcdocs/1-ModAPI/%E6%8E%A5%E5%8F%A3/%E7%8E%A9%E5%AE%B6/%E6%B8%B2%E6%9F%93.html?key=AddPlayerGeometry&docindex=1&type=0#rebuildplayerrender>)，修改其参数。
```python
    def __init__(self, namespace, systemName):
        ClientSystem.__init__(self, namespace, systemName)
        self.ListenForEvent(clientApi.GetEngineNamespace(), clientApi.GetEngineSystemName(), "OnLocalPlayerStopLoading", self, self.OnStopLoading)
    
    def OnStopLoading(self, args):
        playerId = args.get("playerId")
        # 更换模型贴图
        actorRenderComp = clientApi.GetEngineCompFactory().CreateActorRender(playerId)
        actorRenderComp.AddPlayerGeometry('default', "geometry.player")
        actorRenderComp.AddPlayerTexture('default', "textures/entity/player")
        actorRenderComp.RebuildPlayerRender()
    
    def Destroy(self):
        self.UnListenForEvent(clientApi.GetEngineNamespace(), clientApi.GetEngineSystemName(), "OnLocalPlayerStopLoading", self, self.OnStopLoading)
```
> 在这里所调用的接口，实际上是为单独的实体修改了它的客户端实体json，并重建。自定义模型以及json详细参数详见： [自定义生物 (opens new window)](<https://mc.163.com/dev/mcmanual/mc-dev/mcguide/20-%E7%8E%A9%E6%B3%95%E5%BC%80%E5%8F%91/15-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B8%B8%E6%88%8F%E5%86%85%E5%AE%B9/3-%E8%87%AA%E5%AE%9A%E4%B9%89%E7%94%9F%E7%89%A9/01-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%9F%BA%E7%A1%80%E7%94%9F%E7%89%A9.html>)
> 
> 因此只能客户端本地看到这个修改，如果需要让**所有玩家都看到** 新模型，那么需要借助服务器**广播给所有客户端** ，让其他客户端也为这个实体修改参数并重建数据渲染器。
> 
> 同理，如果需要为不同玩家设置不同模型，也可以由服务端来控制，什么玩家显示什么模型。并在新玩家加入时给所有在线客户端广播，通知其他客户端修改新加入的玩家的模型。

进入游戏后，可以看到本地玩家的模型被修改。

![](/dev/mcmanual/mc-dev/assets/img/14.bbab5440.png)

## 本地实体模型的修改

仍继续使用官方Demo中的模型资源，复制下方资源文件到testModel插件的对应资源包目录：

  * `SpigotDemo\CustomPigModelDemo\CustomPigModelClientMod\resource_packs\resource_pack_pig_model\textures\entity\pig` 贴图文件
  * `SpigotDemo\CustomPigModelDemo\CustomPigModelClientMod\resource_packs\resource_pack_pig_model\render_controllers` 渲染控制器
  * `SpigotDemo\CustomPigModelDemo\CustomPigModelClientMod\resource_packs\resource_pack_pig_model\entity` 客户端实体定义


修改的思路如下：

  1. 修改客户端实体定义，添加贴图文件引用。修改渲染控制器
```python
    "textures": {
        "default": "textures/entity/pig/pig",
        "saddled": "textures/entity/pig/pig_saddle",
        // 黄猪贴图引用
        "default_yellow": "textures/entity/pig/yellow_pig",
        // 黄猪上鞍贴图引用
        "saddled_yellow": "textures/entity/pig/yellow_pig_saddle"
    },
```
```python
    // 替换成自定义的猪渲染控制器，可变成黄猪
    "render_controllers": [ "controller.render.pig_custom" ],
```
  2. 修改渲染控制器，根据molang选择贴图的使用。


> molang的解释参考[Wiki (opens new window)](<https://zh.minecraft.wiki/w/Molang>)
```python
    // 自定义猪渲染器，增加黄猪皮肤
    "controller.render.pig_custom": {
        "arrays": {
            "textures": {
                "Array.skins": [ "Texture.default", "Texture.saddled" ],
                "Array.skins_custom": ["Texture.default_yellow", "Texture.saddled_yellow"]
            }
        },
        "geometry": "Geometry.default",
        "materials": [ { "*": "Material.default" } ],
        // 在Python脚本内注册query.mod.custom_pig节点，默认值为0.0。该处表达式意思为：当某只猪的实例的query自定义节点不为1时，则皮肤组切换至黄猪组。
        "textures": [ "query.mod.custom_pig == 0 ? Array.skins[query.is_saddled] : Array.skins_custom[query.is_saddled]" ]
    }
```
  3. Python脚本注册molang节点，通过脚本控制molang的值，来显示不同的模型或贴图。


在修改玩家模型的客户端的代码基础上做修改，框选部分为新增部分：

![](/dev/mcmanual/mc-dev/assets/img/15.a6ff56ab.png)

同样 如果需要指定某个实体ID为某个模型，则需要通过服务端存储实体ID和对应模型之间的映射关系，并广播给所有客户端对指定实体进行molang值的修改。

进入游戏生成猪，可以看到随机的猪的颜色。

![](/dev/mcmanual/mc-dev/assets/img/16.04652296.png)