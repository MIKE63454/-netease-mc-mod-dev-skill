# 实体预设EntityPreset

## 概述

  * 继承关系


  * 描述

EntityPreset（实体预设）是一类特殊的预设，实体预设通常会绑定MC的某类实体，实体预设实例与MC的某一个实体绑定，因此可以使用实体预设来进行一些实体相关的逻辑的编程。如果玩家同时启用了多个AddOn，且这些AddOn中均包含与同一种MC原版实体绑定的实体预设，那么只会加载第一个这种实体预设。

  * 成员变量

变量名 | 数据类型 | 说明  
---|---|---  
engineTypeStr | str | 实体的类型ID  
entityId | str | 实体ID  
updateTransformInterval | int | 实体预设子节点的变换更新间隔，0表示永不更新