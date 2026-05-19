# 自定义生物模型详解

[示例Demo](</dev/mcmanual/mc-dev/mcguide/27-手机网络游戏/课程10：使用Spigot开服/99-下载内容.html#示例demo>)中的包含CustomHumanModelDemo和CustomPigModelDemo。其中

修改玩家模型的demo为：CustomHumanModelDemo

  * 修改玩家模型


修改原版猪生物模型的demo为：CustomPigModelClientMod 由于修改猪为存客户端逻辑，因此不需要Spigot插件即可

  * 50%几率生成黄皮肤猪（修改原生生物模型）


## 修改玩家模型流程

  * 自定义模型以及json详细参数详见： [自定义生物](</dev/mcmanual/mc-dev/mcguide/20-玩法开发/15-自定义游戏内容/3-自定义生物/01-自定义基础生物.html>)

  * 具体生效逻辑为： ![示例1](/dev/mcmanual/mc-dev/assets/img/customModel2.223c3490.png)

  * 最终效果图如下： ![示例2](/dev/mcmanual/mc-dev/assets/img/plugin14.1082ba23.png)


## 修改原生猪模型流程

  * 具体生效逻辑为： 
    * 初始化molang变量

    * 当生成生物时，根据生物entityid创建molang变量，最终赋值

![示例3](/dev/mcmanual/mc-dev/assets/img/customModel3.00c993a2.png)

    * 赋值后，由于自定义猪的render_controller中根据猪molang值判断，若符合条件，则为变色猪

  * 最终效果如下： ![示例1](/dev/mcmanual/mc-dev/assets/img/customModel1.e7765954.png)


入门

40分钟