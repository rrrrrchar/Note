## Bolt的基本概念

### Machine（机）

Machine（机）是我们添加给游戏对象（Game Object）的Bolt组件，相当于一个完整的脚本。Bolt有两种Machine：Flow Machine（流机）和State Machine（状态机）。前者是用一个个Unit串成完整的流式逻辑，后者则是用流机来描述一个个不同的状态（states）以及状态与状态之间转换的条件（Transition）。

### Embed VS. Macro

Machine可以内嵌（Embed）于Unity场景，也可以作为宏（Macro）保存在Asset文件夹里作为一个可以重复使用的游戏资源。

![img](https://tuchuang-1307882010.cos.ap-shanghai.myqcloud.com/img/124473-35d413d35b6280d3.png)

建议最好在刚创建完Machine就决定究竟是使用哪种方式，因为虽然可以从一种方式转换为另一种方式，但这个过程并不是100%无损的，复杂的Graph几乎不可能不在转换后出现各种各样的问题。

我个人的建议是，如果一个Machine所代表的功能会被用在不同的地方，就请尽量使用Macro，或者说，如果你准备“复制/粘贴”某个Machine中的Graph到一个新的Machine中去，那么就将这个Machine转换成Macro，然后在新Machine中调用。

### Graph（图）

在Bolt中，Graph（图）是程序命令执行逻辑的可视化呈现。Flow Machine中呈现的是Flow Graph，State Machine中呈现的是State Graph。

Flow Graph呈现出来的是一堆彼此相连的units（单元），其中必定有一个起点，这个起点通常是一个Event类型的unit，比如常见的`Start`和`Update`。代表着“当……发生时，开始执行本Graph”。



## Unit（单元）

### 单元的类型

Unit是Bolt中最基本的元素，一个unit代表一个操作命令，多个unit按照顺序组合成Graph，从而实现某一个特定的程序功能。默认情况下，Bolt有超过23000个不同的unit，我们可以把它们分成几个大的类别：

- 事件单元（events）：决定“当……发生时”的各种unit，通常都会显示为绿色，被用在一个Flow Graph的起点
- 命令单元（actions）：决定“做什么”的各种unit
- 数据单元（data）：输入各种数据的unit，比如各种以“Literal”结尾的unit。这些单元可以无需调用变量而获得一个Unity数据，比如浮点数（float）、字符串（string）、光线（ray）、层遮罩（layer mask）等
- 计算单元（calculation）：用来处理数据、计算数据的各种unit，比如加减乘除，各种数学函数等
- 变量单元（variable）：用来调用或修改变量的unit，主要是`Set Variable`和`Get Variable`两个
- 逻辑单元（logic）：用来控制Graph的运行逻辑走向的单元，比如`Branch`（相当于“if... else...”条件语句）和`Loop`（相当于“for...”循环语句）等等

### 新建/替换单元

在Graph窗口的空白处点击鼠标右键，选择`Add Unit...`，就可以通过搜索关键字来创建新的unit。如果在一个已有的unit上点击鼠标右键，选择`Replace...`，可以替换这个unit。

- 新建Unit

![img](https:////upload-images.jianshu.io/upload_images/124473-483e01572813ebd5.png?imageMogr2/auto-orient/strip|imageView2/2/w/164/format/webp)

- 替换Unit

![img](https:////upload-images.jianshu.io/upload_images/124473-2f9522d112b336e1.png?imageMogr2/auto-orient/strip|imageView2/2/w/234/format/webp)

### Fuzzy Finder（单元搜索器）

用来搜索unit的窗口叫“Fuzzy Finder”，目前还是比较好用的，搜索速度也比较快。但要注意的是，如果用多个关键字搜索，这多个关键字的顺序必须和结果中这些关键字的出现顺序保持一致才行，比如我搜“position” + “transform”就得不到“transform.position”，必须搜“transform” + ”position”才行。

![img](https:////upload-images.jianshu.io/upload_images/124473-e789707be4c824a4.png?imageMogr2/auto-orient/strip|imageView2/2/w/308/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/124473-c3326779f3a44268.png?imageMogr2/auto-orient/strip|imageView2/2/w/442/format/webp)

### Connections（连接） & Ports（接口）

![img](https:////upload-images.jianshu.io/upload_images/124473-77ca749cb017440b.png?imageMogr2/auto-orient/strip|imageView2/2/w/184/format/webp)

- 三角形线框图标指向的（绿色的宽体箭头）叫Connections（连接），是用来连接不同units以决定其执行顺序的端口
- 圆形线框图标指向的（其他各种）叫Ports（接口），是用来传递数据的端口，根据数据类型的不同，有不同的普调样式，左边的是接受其他数据的接口，右边的是输出数据的接口

并不是每个unit都需要用被Connection相连接才能起效，有的unit根本就没有Connection端口。一般来说，执行计算任务的unit都不需要连接Connection，比如下图：

![img](https:////upload-images.jianshu.io/upload_images/124473-e26d4a2158d2a458.png?imageMogr2/auto-orient/strip|imageView2/2/w/901/format/webp)

当然，这个图里面输出的数据最终还是需要被传递给某个连接了Connection的unit才会真的被执行。

![img](https:////upload-images.jianshu.io/upload_images/124473-25727a9e0b7b17ea.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### Overloads（分身）

一个Unity命令可以有多种调用方式，叫做Overloads。Bolt中使用不同的unit来对应各个Overloads，我个人将其翻译成“分身”。

![img](https:////upload-images.jianshu.io/upload_images/124473-bc9762d97621647f.png?imageMogr2/auto-orient/strip|imageView2/2/w/433/format/webp)

c#脚本中的Overloads

![img](https:////upload-images.jianshu.io/upload_images/124473-cc492ee7ed16325a.png?imageMogr2/auto-orient/strip|imageView2/2/w/581/format/webp)

对应Bolt Unit的Overloads

## Variables（变量）

在Bolt中，有5种不同形式的变量：

- 图示变量（Graph）：图示变量是图示例中的局部变量。它们具有最小的可调用范围，不能在其图示之外访问或修改。
- 对象变量（Object）：对象变量属于游戏对象（GameObject）。他们在该游戏对象的所有图示上共享。
- 场景变量（Scene）：场景变量在本场景的所有图示上共享。
- 应用变量（Application）：即使场景改变，应用变量仍然存在。一旦应用程序退出，它们将被重置。
- 存储变量（Saved）：即使应用程序退出，存储变量也会也会继续存在。他们可以被用作一个简单但功能强大的存储系统。它们被保存在Unity的玩家选项，这意味着它们不能像游戏对象和组件那样被引用。



