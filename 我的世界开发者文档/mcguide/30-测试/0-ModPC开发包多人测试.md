# 用 Mod PC 开发包进行局域网多人测试

在这篇教程中，你将了解如何在我的世界开发工作台(MC Studio) 中启动 Mod PC 开发包，进行同一局域网内的多人联机测试。

## 一、安装我的世界开发工作台(MC Studio)

因为多人测试需要通过官方编辑器进入，你需要：

  * 一个开发者账号
  * 在电脑上安装好 我的世界开发工作台


  1. 访问 [我的世界开发者官网 (opens new window)](<https://mc.163.com/dev/index.html>)，将页面滚动到中间，直至能看到 我的世界开发工作台(MC Studio) 的部分：


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled.72531991.png)

  2. 点击【下载工具】，下载并安装软件，安装完成后，就可以进入下一步了。


## 二、局域网联机测试的两种情况和步骤：
```python
        注：文中的 "A"，"B"仅为便于理解所加。
    A. 单个开发者在单台设备进行联机测试
        1. 运行当前编辑器正在开发的Mod，此时会启动一个【Mod PC开发包A】
        2. 在编辑器中启动一个新的【Mod PC开发包B】
        3. 在【Mod PC开发包B】的【好友界面】找到并加入步骤1中启动的【Mod PC开发包A】
        4. 进行多人联机测试
    
    B. 多个开发者多台设备进行多人联机测试
        1. 开发者A运行当前编辑器正在开发的Mod，此时会启动一个【ModPC开发包A】
        2. 同一局域网内的开发者B在编辑器中启动一个新的【Mod PC开发包B】
        3. 开发者B在【Mod PC开发包B】的【好友界面】找到并加入开发者A启动的【Mod PC开发包A】
        4. 开发者A和开发者B进行多人联机测试
```
上述两种步骤的本质都是：
```python
    1. 在编辑器中点击运行，启动【Mod PC开发包A】（与单机测试相同）
    2. 根据不同情况选择在自己电脑或另一位开发者的电脑上启动一个新的【Mod PC开发包B】
    3. 通过局域网的好友联机功能进行多人测试
```
## 三、在编辑器中运行当前Mod

首先我们在编辑器中点击右上角的运行，启动Mod PC开发包A对我们的Addon或玩法地图进行测试，这一步骤与本地单机测试一样，只需要点击运行，等待Mod PC开发包启动并自动进入游戏即可： ![Untitled](/dev/mcmanual/mc-dev/assets/img/startgame.d78c7b8d.png)

## 四、启动新的 Mod PC 开发包
```python
    注：根据步骤二中的不同情况选择在自己电脑或另一位开发者的电脑上进行本步骤操作
```
  1. 打开【MC Studio主界面】，进入【作品库】，能看到右上角有个【工具箱】按钮：


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled1.7c65b9fb.png)

  2. 点击【工具箱】，选择【Mod PC开发包】，点击启动 MC 游戏：


![Untitled](data:image/png;base64,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)

  3. 此时会弹出版本选择窗口，点击最新的稳定版即可：


![Untitled](/dev/mcmanual/mc-dev/assets/img/chooseversion.fd7691ab.png)

等待加载一段时间后，你最终会看到游戏的主界面：

![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled4.01364297.png)

## 五、找到并进入其他人的局域网世界

  1. 在主界面，点击【游戏】：


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled5.0cb55086.png)

  2. 在顶部的分页中，找到【好友】，点击打开：


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled6.c683d4e0.png)

  3. 在好友界面中，你能看到同一局域网下其他人的游戏，在其中找到要测试的地图名，点击进入游戏：


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled7.a0bc9097.png)

  4. 如果弹窗询问是否下载行为包和资源包，是因为房主所在的游戏加载了额外的资源，点击【下载并加入】


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled8.82dec128.png)

  5. 加载一段时间进入游戏后，在 Esc 打开的菜单中，你应该可以看到游戏内的自己和其他玩家，此时便可以开始进行多人联机测试了


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled9.13bc5a4b.png)

## 六、修改测试选项

在启动器设置可以修改部分ModPC开发包测试时的选项，如下图

![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled10.9e3b67bc.png)

  1. 测试玩家名称：即修改开发包测试时玩家的名称，如图


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled11.348bb937.png)

> 这里的设置暂时只对通过工具箱打开的ModPC开发包生效，开发测试和编辑器内的运行测试暂不生效

  2. 清除缓存：即清除开发包的缓存，使得开发包客户端与服务端一致

  3. 自定义分辨率：即修改开发包启动时的分辨率，在多分辨率适配时，可通过此处设置固定分辨率进行测试，如图


![Untitled](/dev/mcmanual/mc-dev/assets/img/Untitled12.703526d5.png)

https://nie.res.netease.com/r/pic/20220408/b1b536bb-fc51-450d-99d9-648585f79bb9.png

入门

10分钟

true