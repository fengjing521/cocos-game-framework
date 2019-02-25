# cocos游戏轻量级框架
* 以个人实践为主，某些处理可能并非是最佳实践。我刚入门游戏开发，还在不断学习中。
* 个人游戏开发博客，欢迎交流。https://www.yuque.com/fengyong/game-develop-road

###### 重要更新
* 2019-1-30，更新项目名称为cocos-game-framework，更新一些资源的命名方式

### 适用范围
* cocos-creator版本：2.0.7。
* 适用于新手，包含一些基础的游戏界面、逻辑、音频、本地存储、多语言管理。
* 适用于轻量级游戏，单一场景，界面使用prefab进行管理。

### 文件说明
- [**AppMain**] 游戏启动唯一主入口，完成资源初始化，部分单例脚本初始化，本地存储初始化，loading界面逻辑管理
- [**GamePlay**] 游戏的主Gameplay
- [**G**] 通用方法
- [**L**] 本地存储方法
- [**M系列**] 管理组
    - [**MPanel**] 界面管理，界面UI动画管理
    - [**MRes**] 资源管理，动态载入resource下的资源
    - [**MSoud**] 声音管理
    - [**Mi18n**] 多语言管理
    - [**MAction**] 动画管理
- [**T-系列**] 工具组，一般需要挂载到节点上
    - [**TAddPrefab**] onLoad()时添加一个prefab
    - [**TChildNode**] 子节点管理
    - [**TNull**] 空脚本
    - [**TSize**] 节点比例化修改大小
    - [**TZIndex**] 编辑器中修改节点zIndex
    - [**TSimpleFSM**] 简单状态机调度
    - [**TErase**] 橡皮擦效果
    - [**TColor**] 颜色控制工具
    - [**TFollow**] 节点跟随工具
- [**Panel-系列**] 界面组，脚本命名方式为Panel*，需要挂载在界面的同名prefab下
    - [**PanelLoading**] 开场时的loading页面
    - [**PanelBase**] 标准页面（建议新建panel时直接复制PanelBase.prefab和PanelBase.ts并重命名）
    - [**PanelWait**] 一个通用的等待页面
    - [**PanelGuide**] 一个个人使用的新手引导页面
    - [**PanelMessage**] 一个通用的消息页面
- [**Action-系列**] 动画组，脚本命名方式为Anima*，包含一些游戏中常用的复杂动画
- [**S-系列**] 子系统组，脚本命名方式为S*，游戏子系统管理
- [**C-系列**] 控制器组，脚本命名方式为C*，游戏中重要组件的控制器

### 两层逻辑设计（尽量使得逻辑扁平话，少分层，多分模块）：System+Controller
1. System层，包含游戏主逻辑（GamePlay）和各子系统（System）
    * 主要实现游戏逻辑（可以理解成不同组件的操作集合），比如过关逻辑（next_level）就包含清理数据，场景切换，加分动画等子逻辑。
    * 子系统则用来实现除游戏主逻辑外的额外逻辑，例如：新手引导系统、离线奖励系统等。
    * 子系统根据控制子逻辑的多少分成3类：对应0个Panel的子系统，如数值系统；对应1个固定Panel的子系统，如离线奖励系统；对应多个不固定Panel的子系统，如分数系统、收集系统等。
    * 对应1个固定Panel的子系统，如离线奖励系统，进行脚本简化，将系统脚本（S*）合并到界面脚本中（Panel*）。
2. Controller层，包含界面的控制器（Panel）和各界面中重要组件的控制器（Controller）