# C#



# Component

## 动画系统

### Animation Clip

对物体变化情况的一种展示



### Animator Controller

跟踪当前动画的播放状态，并可以根据设置切换状态。

### Avatar

为了复用Humanoid动画

### Animator Component

- Animator Controller



- avatar



- ApplyRootMotion



- updateMode

Normal  与帧同步 update

Animate physics 与物理帧同步

Unscaled Time  与帧同步，忽略时间标尺

- CullingMode

剔除模式

Always Animate

不管相机，该怎么计算就怎么计算

Cull Update Transform

只计算状态，会剔除IK之类的

Cull Completely

完全停止动画，等到被看到

### Animator

动画状态机

控制多个动画文件播放和切换的工具

- 动画状态

可以分三类

​	单独的动画状态

​	混合树

​	另一个状态机

- Layers



- Parameters

  

# Plugin



## Input System



## Timeline

多物体协同动画

一个窗口，提供了多个轨道，每个轨道可以单独编辑。轨道之间可以排列和融合



协同动画的enity上要有animator组件



定义：

激活轨道，一个轨道只能控制一个对象的激活与否





Animation Track

Audio Track





流程：

1. 创建timeline
2. 

## CineMachine

 相机管理工具（也可以叫虚拟相机）

2016-2017年和Timeline一起推出的功能

能平滑切换不同相机，调整相机的参数























































