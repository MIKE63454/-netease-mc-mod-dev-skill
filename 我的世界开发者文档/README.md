# 网易我的世界开发者文档

从 [网易我的世界开发者官网](https://mc.163.com/dev/apidocs.html) 爬取的完整开发者文档。

## 文档统计

| 板块 | 说明 | 页数 |
|------|------|------|
| **mcdocs** | ModAPI / Apollo / PresetAPI 接口文档 | 269 |
| **mcguide** | 开发指南（入门教程、功能文档） | 565 |
| **mconline** | 教学课程（Addon教程、网络服教程等） | 614 |
| **总计** | | **1,448** |

## 目录结构

```
api_docs/
├── mcdocs/          # API 接口文档
│   ├── 0-概述/
│   ├── 1-ModAPI/     # ModAPI (Python SDK)
│   │   ├── 接口/通用/  # Component, System, 事件, 设备, 存储, 数学, 工具, 调试
│   │   ├── 接口/世界/  # 地图, 实体管理, 方块管理, 生物生成, 配方, 渲染, 时间, 天气, ...
│   │   ├── 接口/实体/  # 实体类型, 附加值, 属性, 行为, 状态效果, 渲染, 背包, ...
│   │   ├── 接口/玩家/  # 属性, 行为, 渲染, 背包, 摄像机, 动画, 游戏模式, 权限, 导航
│   │   ├── 接口/方块/  # 方块状态, 属性, 方块实体, 几何体模型, 渲染, 容器, 红石, ...
│   │   ├── 接口/特效/  # 通用, 文字面板, 序列帧, 粒子, 模型特效, 微软粒子
│   │   ├── 接口/自定义UI/ # 通用, 自定义书本, 自定义成就, UI界面, UI控件
│   │   ├── 接口/虚拟世界/ # 世界, 相机, 模型, 其它对象
│   │   ├── 接口/后处理/  # 渐晕, 模糊, 色彩, 镜头效果, 自定义
│   │   ├── 事件/        # 物理, 世界, 实体, 玩家, 方块, 物品, 模型, UI, 音效, ...
│   │   ├── 枚举值/      # EntityType, EffectType, GameType, ... (70+ 枚举)
│   │   └── 更新信息/    # 版本更新日志 (1.21 ~ 3.8)
│   ├── 2-Apollo/      # Apollo 服务端插件
│   │   └── 4-SDK/      # 大厅, 游戏服, 控制服, 功能服, 公共API
│   └── 3-PresetAPI/   # 预设和零件 API
│       ├── 预设管理/
│       ├── 预设对象/通用/ # GameObject, Transform, BoxData, SdkInterface
│       ├── 预设对象/预设/ # EntityPreset, EffectPreset, PlayerPreset, UIPreset, ...
│       └── 预设对象/零件/ # PartBase, TriggerPart, WorldPart, PlayerBasicPart, ...
├── mcguide/          # 开发指南
│   └── (565个页面，涵盖入门教程、功能文档、美术教程、手机网络游戏等)
├── mconline/         # 教学课程
│   └── (614个页面，涵盖Addon教程、网络服教程、创造营教程等)
└── README.md
```

## 文档来源

- 官方文档: https://mc.163.com/dev/apidocs.html
- 开发指南: https://mc.163.com/dev/mcmanual/mc-dev/mcguide/
- API 文档: https://mc.163.com/dev/mcmanual/mc-dev/mcdocs/
- 教学课程: https://mc.163.com/dev/mcmanual/mc-dev/mconline/

## 爬取时间

2026-05-19
