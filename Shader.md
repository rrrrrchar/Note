# UnityShader入门精要

## 1.杂七杂八的

### 1.1OpenGL/DirectX

都是各类硬件底层的进一步封装，一个接口

### 1.2HLSL、GLSL、Cg

基于接口的编程语言

### 1.3什么是Shader

1. GPU流水线上一些高度可编程的阶段
2. 有特定类型的着色器（顶点和片元）
3. 依靠着色器控制渲染细节

## 2.渲染流水线

### 2.1定义

渲染流水线=应用阶段+几何阶段+光栅化阶段

### 2.2应用阶段

应用阶段 = CPU和GPU之间的通信

1. 把数据加载到显存中
2. 设置渲染状态
3. 调用DrawCall

#### 2.2.1把数据加载到显存中

数据：

硬盘→内存→显存

#### 2.2.2设置渲染状态

1. 哪些需要渲染
2. 渲染的属性是什么
   1. 使用哪个着色器（顶点 or 片元）
   2. 光源属性
   3. 材质

#### 2.2.3调用DrawCall

DrawCall就是一个命令



### 2.3GPU流水线

#### 2.3.1定义

GPU流水线=几何阶段+光栅化阶段

几何阶段=顶点着色+裁剪+屏幕映射

光栅化阶段=三角形设置+三角形便利+片元着色+逐片元操作

#### 2.3.2顶点着色器

**坐标转化**



**逐顶点光照**

#### 2.3.3裁剪

完全在相机视野外的不渲染

部分在相机视野外的进行裁剪后渲染

全部在相机视野内的都进行渲染

#### 2.3.4屏幕映射

三维坐标转二维



#### 2.3.5三角形设置

从点计算到面

#### 2.3.6三角形遍历

计算出每个像素点的状态

片元序列

#### 2.3.7片元着色器

通过片元序列开始计算出每个像素点的颜色值

#### 2.3.8逐片元操作

决定每个片元的可见性以及测试



## 3.Unity Shader基础

### 3.1概述

#### 3.1.1unity shader流程

1. 创建Material
2. 创建一个unity shader，把shader赋给Material
3. 把Material赋给要渲染的对象
4. 在材质面板调整Unity Shader的属性

#### 3.1.2材质

Material需要结合Mesh或者 Paricles System组件来工作



### 3.2 ShaderLab

所有的Unity Shader 都是用ShaderLab编写的

### 3.3ShaderLab的结构

1. Shader名
2. Property
   1. 名字
   2. 类型
   3. 变量默认值
3. SubShader
   1. RenderSetup
   2. Tags
   3. PASS

| TagType | TagDesc | TagSample |
| ------- | ------- | --------- |
|         |         |           |
|         |         |           |
|         |         |           |
|         |         |           |
|         |         |           |
|         |         |           |
|         |         |           |



### 3.3表面着色

灵活但是半自动，依靠Unity支持较多

### 3.4顶点着色

复杂但是灵活

### 3.5固定函数着色器

不用了



## 4.数学基础

### 4.2坐标系

1. 笛卡尔坐标系
   1. 二维
   2. 三维
2. 左手坐标系
   1. 大拇指x
   2. 食指y
   3. 中指z
3. 右手坐标系
   1. 同上



Unity使用的坐标系：

左手坐标系

### 4.3点和矢量

点 是n维空间的一个位置

矢量= 长度+方向

标量=长度

#### 4.3.1运算

- 矢量，标量
  - 

- 矢量，矢量
  - 点积
    - 结果是标量
    - a · b = ax * bx + ay * by + az * bz 
    - a · b = b · a
    - 几何意义：
  - 叉积
    - 结果是矢量
    - a * b = ( ay * bz - az * by , az * bx - ax * bz , ax * by - bx * ay )
    - a * b  = - b * a
    - 几何意义 ：得到垂直于 a b 的矢量 z

- 矢量
  - 



|            | 运算 | 备注       |
| ---------- | ---- | ---------- |
| 矢量，标量 | 乘除 |            |
| 矢量，矢量 | 加减 |            |
| 矢量       | 求模 |            |
|            | 点积 | 轴相乘 dot |
|            | 叉积 |            |

### 4.4矩阵

和矢量联系起来

- 乘法

A(m,1) * B(1,n) =C(m,n)

- 转置矩阵
- 逆矩阵

单位矩阵的逆矩阵是他本身

转置矩阵的逆矩阵 == 逆矩阵的转置

- 正交矩阵

正交矩阵 = （矩阵A *  转置矩阵A  = 单位矩阵） =  （ 转置矩阵  = 逆矩阵）



## 5.开始学习ShaderLAB

```
Shader "Custom/5-2"
{
    Properties
    {
        _Color("Color Tint",Color)=(1,1,1,1)
    }
    SubShader
    {
        Pass
       {
            CGPROGRAM
            //顶点
            #pragma vertex vert
            //片元
            #pragma fragment frag
            //
            uniform fixed4 _Color;
            //
            struct a2v{
                float4 vertex: POSITION;
                float3 normal: NORMAL;
                float4 texcoord: TEXCOORD0;
            };

            struct v2f{
                float4 pos: SV_POSITION;
                fixed3 color: COLOR0;
                };
            v2f vert(a2v v)
            {
                v2f o;
                o.pos=UnityObjectToClipPos(v.vertex);
                o.color=v.normal *0.5 +fixed3(0.5,0.5,0.5);
                return o;
            }
            //
            fixed4 frag(v2f i):SV_Target{
                fixed3 c=i.color;
                c*=_Color.rgb;
                return fixed4(c,1);
            }
            ENDCG

        }
    }
    FallBack "Diffuse"
}

```





### CG/HLSL语义



## 6.基础光照

宏观上渲染包含了两部分：像素的可见性，像素的光照计算

 通过 辐照度 量化光

辐照度 = d / cos （setae）



折射 ， 透射 ， 散射  ——> 高光反射，漫反射

标准光照模型 BRDF



## 7.基础纹理

Blinn-Phong光照模型

```

```



## 8.透明效果





## 9.更复杂的光照



# JTRP

