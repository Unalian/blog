---

date: 2020/2/19
tags:
- 自学笔记
- u3d
categories:
- 游戏及图形学
- unity
---
[自学视频](https://www.bilibili.com/video/av10281109?p=1)
# Unity

#### 1.1窗口界面分类

![窗口](https://img.cetacis.dev/uploads/2020/02/18/Screen-Shot-2020-02-18-at-5.02.49-PM.png)

* Game

即玩家可见的画面。

在游戏运行时对场景的改变不会存盘。

* 右上角可改变layout
<!-- more -->
#### 1.2 菜单界面

![](https://img.cetacis.dev/uploads/2020/02/18/Screen-Shot-2020-02-18-at-5.21.46-PM.png)

#### 1.3 scene与场景漫游

scene

“Q”“W”“E”“R”在场景编辑窗口对窗口与内容进行改变。

双点触控板，ASDWQE对视角进行改变。

在edit->strp setting修改一步的大小

control+旋转/平移=定量旋转/修改

global 全局坐标徐 local 自身坐标系

center/pivot 同时选中两个物体时的坐标系 

![](https://img.cetacis.dev/uploads/2020/02/18/Screen-Shot-2020-02-18-at-7.50.31-PM.png)

![切换23d](https://img.cetacis.dev/uploads/2020/02/18/Screen-Shot-2020-02-18-at-7.55.15-PM.png)

可以点xyz获得视野，也可以shift+中间获得45度视野 

#### 1.4 Hierarchy与场景搭建

1. 基本组件

   game object

   **camera **
   **particle system（粒子系统）**

   * emisson 释放速度
   * shape 释放形状
   * force over time 添加物理效果（坠落感）
   *  external force（风）
   * collision 粒子碰撞
   * render 修改粒子材质

   **GUIs**

2. 灯光组件

   平行光：模拟太阳光
   
   area light：只在最终渲染的时候用到，在场景中没有体现。
   
3. 物体组件

   capsule collider 碰撞检测轮廓 绿色线框代表collider 蓝线为三角面

   mesh filter 材质

4. sprite组件

   多用于展现2d动画资源。3d模拟背景动画。

   将资源silce，apply以后就是sprite。

5. terrain组件 

   地形模型

   创建到场景物体里的terrain会自动在assert中生成一个新的资源。

#### 1.5 窗口操作资源管理

project

对资源修改无法撤销

新建：

* script 脚本
* shader 修饰材质
* Newcomputeshader gpushader
* prefab
* material 材质
* cubmap 立方体贴图 （天空盒）

材质 = texture+shader

**texture** ：UI、Mesh模型、粒子效果、视频资源 、render text（渲染贴图）：将camera捕捉到的东西通过渲染变成texture，比如小地图等

![render texture](https://img.cetacis.dev/uploads/2020/02/19/Screen-Shot-2020-02-19-at-3.10.29-PM.png)

**shader（着色器）**2.5.5

* 标准着色器

* 透明着色器

* 镂空着色器

* 自发光着色器

* 反射着色器

  在材质球处可以选择着色器

  diffuse

   bumped diffuse（texture+normal texture）

  diffuse detail （加细节）

  decal（多种贴图重叠效果）

  ... 

#### 1.6 项目资源管理

* 当导入package，当前文件夹有与导入文件夹重名的文件夹，可能会对文件夹进行合并，造成不必要的麻烦。应该新建文件夹再导入。

* 特殊文件夹命名：

  standard Assets/pro standard assets: 非unity内置包，会被预先加载。

  Editor  拓展编辑器

  Plugin 插件 dll文件

  resources 资源

#### 2.1 Inspector与游戏组件

检视面板：inspecter 属性控制面板

add component（添加组件）2.6.4

effect：

* trail Render（轨迹组件）

  *因为是特效组件，所以必须添加一个partical shader*

* line render

  画线（激光） 

* Lens flare 

  将asserts中的lens flare文件拖入此组件，即可实现太阳光晕效果

* halo 光晕

* projector 投影器

  可以将材质达到plate上，实现对角色的灯光跟随

render：

怎么将效果渲染在摄像机/屏幕上

* camera 将看到的打到game中，或者render texture中

  skybox、flare layer、audio layer、Guilayer

*  Light Prode group 烘焙动态物体

  1. gameobject加Light Prode group组件（设置探针计算光照）
  2. 在物体上添加mesh render组件 并且勾选use light prode。
  3. 烘焙 烘焙之后，一般移动物体光照就不会在变，但是在这里，因为加入了光照探针，所以会变化

* Light 三中光源

* LOPGROUP 不同距离的细节分级

脚本组件

![unity构架](https://img.cetacis.dev/uploads/2020/02/19/Screen-Shot-2020-02-19-at-8.50.05-PM.png)

C# 



![struct](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-12.20.49-PM.png)

![枚举类型：限定变量可能性](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-12.24.17-PM.png)

![枚举类型强制类型转换](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-12.28.11-PM.png)

var 声明

**Class**

 成员变量/方法 实例化 构造函数 

静态方法/ 成员变量直接存储在class中，不需要实例化后调用（实际上，实例化无法调用static的方法/变量）

![属性变量Age](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-2.40.30-PM.png)

*属性变量方便地完成了对变量的getset并且具有安全性*

**interface**

方法属性索引事件

在接口中定义方法，在有一个类链接这个借口，在这个类中完成方法。

![接口](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-2.53.48-PM.png)

**abstract class **

抽象类：只声明方法，然后它被其他类继承，实现它的方法。

![抽象类与类的继承](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-2.57.39-PM.png)

**抽象类和接口区别与共同点**

抽象类归根结底是类，可以声明一个抽象方法但是由其他类继承抽象类来实现这个方法，也可以直接在抽象类里直接实现一个方法，根据父类的性质，它的 子类都可以调用这个方法。

接口则没有类的性质，只能有方法属性索引事件，不能有成员变量字段。

只能继承一个抽象类但是可以继承多个接口

两者都不能被实例化

**权限**

字段一般都是private+setget方法。

或者public属性

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-3.11.06-PM.png)

脚本

**debug信息**

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.02.18-PM.png)

如果在 Update 里，则没一帧都会显示

**脚本生命周期**

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.13.29-PM.png)

* reset 是物体添加组件的时候

* awake是物体存在就会调用

* 当脚本被调用就会调用start

  

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.15.57-PM.png)

* fixupdate ->update ->LateUpdate

  fixupdate是根据物理事件调用，update lateuodate是基于时间调用。

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.23.04-PM.png)

* ondisable 脚本停止
* ondestroy 物体销毁

####  behavior事件相应



**启动刷新函数：**

1. 启动函数

**awake()**

* 初始化函数，游戏开始时系统自动调用
* 一般用来创建变量
* 无论脚本组件是否被激活都被调用

**start()**

* 初始化函数，在awake之后update之前
* 一般给变量赋值
* 只有脚本组件激活时才能被调用

2. 刷新函数

**update()**

* 每一帧调用
* 用于非物理运动（匀速运动）

**FixedUpdate()**

* 固定时间调用一次
* 一般用于物理运动（物理运算）
* edit->time可进行修改

#### 交互函数

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.43.12-PM.png)

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.43.22-PM.png)

**物理**

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.43.50-PM.png)

OnTriggerEnter()

Cube1 :加入box collider组件（碰撞体），勾选trigger。cube1是触发器

![cube1](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.51.24-PM.png)

Cube2:挂脚本，加碰撞体，用rigidbody加重力。当cube1（触发器进入），则cube1 位移

![cube2](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.53.08-PM.png)

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-5.55.21-PM.png)

**OnTriggerStay()**当碰撞体接触触发器时，此函数在每一帧被调用

![](https://img.cetacis.dev/uploads/2020/02/20/Screen-Shot-2020-02-20-at-6.02.26-PM.png)

给cube2挂上脚本后，当cube1触碰cube2，cube1会向上移动。当cube1不加重力，他会一直上升。当它加上重力，当两者分开cube1又会下降。
