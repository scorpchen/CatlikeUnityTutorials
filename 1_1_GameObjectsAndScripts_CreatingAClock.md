# 游戏对象和脚本——创建一个钟

* 用简单的游戏对象拼成一个钟
* 编写写一个 C# 脚本
* 旋转指针以显示时间
* 让指针动起来

这是 Unity 基础教程系列的第一篇。在本篇中，我们会创建一个简单的时钟表盘，并且为它编写一个脚本，让它能够实时显示当前的时间。学习本篇不需要你有任何Unity的经验，但你应该要用过一些有多窗口的应用程序。

在教程 [原网站](https://catlikecoding.com/unity/tutorials/basics/game-objects-and-scripts/) 的底部，你将找到指向教程许可证的链接、包含已完成教程项目的存储库以及教程页面的PDF版本。

本教程是使用 Unity 2020.3.6f1 制作的。

![是时候创建一个时钟了](.\images\game-objects-and-scripts\tutorial-image1-1.jpg)
<center>是时候创建一个时钟了</center>

## 1. 创建项目
在开始使用 Unity 之前，我们必须先创建一个项目。

### 1.1 新项目
当你打开 Unity 时，你将看到 Unity Hub。这是一个启动器，你可以从中创建或打开项目、安装 Unity 版本或者执行其他一些操作。如果你没有安装 Unity 2020.3 或更高版本，请马上添加它。

<br>

> <center>哪些 Unity 版本合适？</center>
> 
> Unity 每年发布多个新版本，有两个并行的发布计划：稳定的 LTS 版本和以 f1 为后缀的最终正式版本。LTS 代表长期支持版，通常情况下 Unity 提供两年的支持。本教程使用的版本是 LTS 2020.3.6。版本号的最后一位表示补丁修补版本，补丁版本包含错误修复，很少有新功能，所以任何 2020.3 版本都可以用于本教程。
> 
> 最高的 Unity 版本是开发版，它引入了新功能并且可能删除旧功能。这些版本不如 LTS 版本可靠和稳定，每个版本仅受支持几个月。

<br>

创建新项目时，你可以选择其 Unity 版本和模板。我们将使用标准的 3D 模板。创建后，它将添加到项目列表中，并在相应版本的 Unity 编辑器中打开。

### 1.2 编辑器布局
如果你没有自定义编辑器，那么打开项目后，Unity 会显示默认的窗口布局。

![默认编辑器布局](.\images\game-objects-and-scripts\default-layout.png)
<center>默认编辑器布局</center>

默认布局已经包含了我们需要的所有窗口，但你也可以根据需要通过对窗口进行重新排序和分组来对其进行自定义。你还可以打开和关闭窗口，例如资源商店的窗口。每个窗口都有自己的配置选项，可以通过右上角的三点按钮访问。除此之外，大多数窗口还有一个带有更多选项的工具栏。如果你的窗口看起来与教程里的不一样（例如，场景窗口具有统一的背景而不是天空盒），那么可能是这些工具栏的选项有不一样的设置。

你可以通过 Unity 编辑器右上角的下拉菜单切换到预配置的布局，也可以将当前布局保存在那里，以便以后可以恢复。

### 1.3 包（Packages）
Unity 的很多功能都是模块化的，除了核心功能之外，还可以下载额外的包并将其导入到项目中。默认的 3D 项目会自动包含一些包，你可以在项目（Project）窗口中的“Packages”目录下看到这些包。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\default-packages.png"/>
</div>
<center>默认的包</center>
<br>

可以点击项目窗口右上角看起来像一只被划掉的眼睛的按钮来隐藏这些包。这只是为了减少编辑器中的视觉混乱，这些包依然被包含在项目里。这个按钮旁边的数字代表有多少个这样的包被包含在了项目中。

你可以通过包管理器（Package Manager）控制项目中要包含哪些包，包管理器可以通过“窗口/包管理器（Window/Package Manager）”打开。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\package-manager.png"/>
</div>
<center>包管理器，仅显示项目中的包。</center>
<br>

这些包给 Unity 添加了额外的功能。例如，Visual Studio Editor 为用来编写代码的 Visual Studio 编辑器添加了集成环境。本教程不使用所有包包含的功能，因此我将它们全部删掉。不过我还是留下了 Visual Studio Editor，因为我们在用 Visual Studio 编写代码的时候需要它。如果你使用的是其他编辑器，那么也要将该编辑器的集成包包含进来，例如 Visual Studio Code 编辑器的 Visual Studio Code Editor Package。

删除包最简单方法是先将包管理器的过滤器设置为“In Project”。然后选择要删除的包，并点击窗口右下角的“删除（Remove）”按钮。Unity 在每次删除后都会重新编译，所以删除需要等几秒钟才能完成。

删除掉除了 Visual Studio Editor 之外的所有内容后，项目里还剩下三个包：Custom NUnit，Test Framework 和 Visual Studio Editor。另外两个也保留下来是因为Visual Studio Editor 依赖于它们。

你可以通过项目设置让依赖项和隐式导入的包在包管理器中可见：通过 “编辑/项目设置...（Edit/Project Settings...）” 打开项目设置窗口，选择 “包管理器（Package Manager）”，然后在 “高级设置（Advanced Settings）” 下启用 “显示依赖项（Show Dependencies）”。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\package-manager-settings.png"/>
</div>
<center>包管理器项目设置：显示已启用的依赖项</center>
<br>

### 1.4 色彩空间
如今，渲染一般是在线性空间中计算的，但 Unity 的默认配置仍然为使用伽玛空间。为了获得最佳的视觉效果，请选择项目设置窗口的“Player”，打开“其他设置（Other Settings）”面板，找到“渲染（Rendering）”部分，确保“色彩空间（Color Space）”设置为“线性（Linear）”。Unity 可能会警告这需要很长时间，但对于空项目来说用不了多久，所以直接确认就好。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\color-space.png"/>
</div>
<center>颜色空间设置为线性</center>
<br>

### 1.5 示例场景（Sample Scene）
新工程包含一个名为 SampleScene 的示例场景，默认情况下会打开该场景。你可以在项目窗口中的“资产（Assets）/场景（Scenes）”下找到它。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\two-column-layout.png"/>
</div>
<center>项目窗口中的示例场景</center>
<br>

默认情况下，项目窗口使用两列布局。你也可以通过窗口右上角的三个点打开菜单，切换到单列布局。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\one-column-layout.png"/>
</div>
<center>单列布局</center>
<br>

示例场景中有一个主摄像机（Main Camera）和一个定向光源（Directional Light），它们都是游戏对象（Game Object），会在层级（Hierarchy）窗口中按照父子节点的关系显示出来。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\sample-scene-hierarchy.png"/>
</div>
<center>场景中的游戏对象</center>
<br>

你可以通过层级（Hierarchy）窗口或者场景（Scene）窗口来选择游戏对象（Game Object）。相机和光源在场景中都会显示成图标的样子，相机的图标是个老式胶片相机，光源的则是个太阳。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\scene-icons.png"/>
</div>
<center>场景窗口中的图标</center>
<br>

<br>

> <center> 如何在场景窗口中自由移动？ </center>
>
> 鼠标右键可以旋转视角；按住鼠标右键不放，使用a、s、w、d键可以在场景视图中前后左右移动；鼠标滚轮可以缩放视图；按 F 键可将视图跳转到当前选定的游戏对象上。还有很多移动方法你可以去网络上查找相关资料。

<br>

选中游戏对象后，有关这个对象的详细信息将显示在监视器（Inspector）窗口中，这个窗口我们后面再说，会经常用到它。我们不需要修改摄像机和灯光，所以可以通过单击层级（Hierarchy）窗口中左侧的眼睛图标将它们隐藏。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hidden-objects.png"/>
</div>
<center>隐藏游戏对象</center>
<br>

<br>

> <center> 眼睛旁边的手状图标是干嘛的？ </center>
> 你可能注意到了隐藏图标旁边有个手的图标，打开这个图标会禁止你在场景窗口中选择该对象。

<br>

## 2. 创建一个简单的时钟
现在我们的项目已经设置完成，可以开始创建时钟了。

### 2.1 创建游戏对象
我们需要一个游戏对象来表示时钟。我们将从最简单的游戏对象——空对象开始。

通过顶部菜单栏的 “游戏对象（Game Object）/创建空对象（Create Empty）”便可以在场景中创建一个空对象，也可以鼠标右键单击层级（Hierarchy）窗口中空白区域，在弹出的菜单中选择 “创建空对象（Create Empty）”。

这样便将游戏对象添加到了场景中，它在层级（Hierarchy）窗口中的 SampleScene 下面，而且已经是选中的状态。此时 SampleScene 会出现星号标记，说明它现在有改动，且没有保存。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\new-game-object.png"/>
</div>
<center>选择了新游戏对象的层级窗口</center>
<br>

只要选中了游戏对象，监视器（Inspector）窗口就会显示游戏对象的详细信息。

监视器（Inspector）窗口顶部是被选中的游戏对象的基本信息，包含了对象的名字以及一些配置选项。默认情况下，对象处于启用状态（Active）、非静态（Static）、未标记（Untagged）且位于默认（Default）图层（Layer）上。我们保持默认设置，只将它的名称改成“Clock”。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\inspector.png"/>
</div>
<center>选择了时钟的监视器窗口</center>
<br>

在基本信息下面是游戏对象所有组件的列表。在 Unity 中，所有游戏对象都会有一个 Transform 组件，并且 Transform 组件始终会在组件列表的顶部，它控制着游戏对象的位置、旋转和缩放。

确保时钟对象的位置（Position）和旋转（Rotation）都设置为 0，其缩放（Scale）都为1。

<br>

> <center> 2D 对象呢？ </center>
>
> 对于 2D 的游戏对象（如UI对象）来说，它们不需要三维的变量，因此有专用的 RectTransform 组件来代替 Transform 组件。

<br>

由于游戏对象为空，因此在场景窗口是看不见这个游戏对象的。但是，操作工具在游戏对象的位置是可见的。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\move-tool.png"/>
</div>
<center>使用移动工具选择</center>
<br>

<br>

> <center> 为什么选择时钟后看不到操作工具？ </center>
>
> 操作工具存在于场景窗口中。确保你查看的是场景（Scene）窗口，而不是游戏（Game）窗口。

<br>

可以通过场景窗口左上角的工具栏选择操作工具。也可以通过 Q、W、E、R、T 和 Y 键来快速选择。默认情况下会选择移动工具。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\manipulation-toolbar.png"/>
</div>
<center>操作工具栏</center>
<br>

工具栏旁边还有三个按钮，第一个用来控制操作工具放置在物体的中心还是放置在轴点上；第二个用来控制操作工具采用世界（Global）坐标的旋转值还是本地（Local）坐标的旋转值；第三个按钮控制连续地还是离散地移动，只在采用移动工具时生效，打开后每次移动只能移动到单位距离整数倍的地方。

### 2.2 创建表盘
我们有了一个时钟对象，但它是空的，我们看不到任何东西。我们必须向其中添加3D模型，以便渲染出一些东西让我们看到。Unity 包含一些原始的 3D 对象，我们可以使用它们来构建简单的时钟。

让我们首先通过 “游戏对象/3D 对象/圆柱体（Game Object/3D Object/Cylinder）” 向场景添加一个圆柱体，确保它的 Transform 与我们的时钟对象具有相同的值。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\cylinder-scene.png"/>
<img src=".\images\game-objects-and-scripts\cylinder-inspector.png"/>
</div>
<center>圆柱体游戏对象的监视面板</center>
<br>

新对象比空的游戏对象多了三个组件。首先，它有一个 MeshFilter 组件，其中包含对 Unity 内置圆柱体网格的引用。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\mesh-filter.png"/>
</div>
<center>MeshFilter 组件，其 Mesh 设置为 “Cylinder”</center>
<br>

第二个是 MeshRenderer，用来渲染 MeshFilter 中的网格。它还控制着用于渲染的材质球，现在用的是默认材质球（Default-Material）。这个材质球也会显示在组件列表的最下方。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\mesh-renderer.png"/>
</div>
<center>MeshRenderer 组件，其 Materials 含有一个“Default-Material”</center>
<br>

第三个是 CapsuleCollider，用于 3D 物理的碰撞检测。该对象是一个圆柱体，但是它的碰撞体（Collider）却是胶囊（Capsule）形状的，因为 Unity 没有内置的圆柱碰撞体。

我们不需要碰撞体，所以我们把它删掉。组件可以通过右上角的三点下拉菜单删除，也可以直接右键点击组件选择移除组件（Remove Component）删除。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\cylinder-without-collider.png"/>
</div>
<center>不带碰撞体的圆柱体对象</center>
<br>

我们减小圆柱体缩放值的 y 分量到 0.2，将其压平，成为时钟的表面。圆柱本来高度是两个单位，所以缩放y值到 0.2 后，它的高度便是 0.4 个单位。让我们做一个大钟，将圆柱体缩放的 X 和 Z 分量增加到 10。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\cylinder-scaled-scene.png"/>
<img src=".\images\game-objects-and-scripts\cylinder-scaled-inspector.png"/>
</div>
<center>缩放后的圆柱体</center>
<br>

我们的时钟现在是水平躺在地上的，我们想要它立起来，所以要将它旋转90度。在 Unity 中，X 轴指向右，Y 轴指向上方，Z 轴指向前方。所以如果我们想沿着 Z 轴看到它的正面，将其沿着 X 轴方向旋转90度即可。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\cylinder-rotated-scene.png"/>
<img src=".\images\game-objects-and-scripts\cylinder-rotated-inspector.png"/>
</div>
<center>旋转后的圆柱体</center>
<br>

将圆柱对象的名字改成 Face，因为它表示时钟的表面。它只是时钟的一部分，所以我们在层级（Hierarchy）窗口把它拖到时钟节点的下面，让它成为 Clock 对象的子对象。

子对象会受父对象 Transform 的影响，当父对象改变位置、旋转、缩放时，子对象也会相应改变，就好像它们是一个单一的对象一样。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\child-object.png"/>
</div>
<center>Face 子对象</center>
<br>

### 2.3 创建刻度
时钟的表盘上会有用来显示时间的刻度，一般是 12 小时制。

我们新建一个立方体对象（Game Object/3D Object/Cube），将其命名为 Hour Indicator 12，并将其拖入时钟节点下面，成为时钟对象的子项。子对象在层级窗口中的顺序不重要，把它放在 Face 对象的上面或下面都行。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hour-indicator-child.png"/>
</div>
<center>刻度子对象</center>
<br>

将刻度对象缩放（Scale）值的 X 设置为 0.5，Y 设置为 1，Z 设置为 0.1，让它变成一个狭窄的扁平块。然后将其位置（Position）的 X 设置为 0，Y 设置为 4，Z 设置为 −0.25，让它移动到表盘顶部，表示 12 点。同时删除其 BoxCollider 组件。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\indicator-scene.png"/>
<img src=".\images\game-objects-and-scripts\indicator-inspector.png"/>
</div>
<center>12点处的刻度</center>
<br>

现在刻度和表盘是一个颜色，所以几乎看不到刻度。要改变它的颜色，我们需要为它单独创建一个材质球。在 Project 窗口里，通过资产/创建/材质（Assets/Create/Material）创建一个材质球，命名为 Hour Indicator。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\material-project-one-column.png"/>
<img src=".\images\game-objects-and-scripts\material-project-two-column.png"/>
</div>
<center>项目窗口中的 Hour Indicator，单列布局和双列布局</center>
<br>

选中这个材质，我们可以在监视（Inspector）面板中看到它的相关信息。要更改材质球的颜色，我们单击反照率（Albedo）右边的颜色选框，并选择你喜欢的颜色。我选择了深灰色，对应于十六进制（Hexadecimal）的颜色码是 #494949 。我们不使用 alpha 通道，因此它的值无关紧要。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hour-indicator-material.png"/>
</div>
<center>深灰色的 albedo </center>
<br>

<br>

> <center> 什么是反照率（Albedo）？ </center>
> 
> 反照率是一个拉丁词，意思是白色。它是被白光照亮时某物的颜色。

<br>

我们把这个材质赋给刻度对象，可以直接将 Project 窗口中的材质拖到场景中的刻度对象（Hour Indicator 12）上，也可以选中刻度对象，在监视器面板找到其 Mesh Render组件，将材质拖入其 Materials 属性中。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\dark-hour-indicator-scene.png"/>
<img src=".\images\game-objects-and-scripts\dark-hour-indicator-inspector.png"/>
</div>
<center>灰色的刻度</center>
<br>

### 2.4 十二小时刻度
我们要创建十二个这样的刻度，用来表示12个小时。

首先调整视角方向，单击场景视图右上角负 Z 轴的圆锥体，使视角调整成沿 Z 轴直视的方向。单击场景左上角的网格工具，可以将网格平面调整到 Z 轴。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\looking-along-z-axis.png"/>
</div>
<center>沿着 Z 轴直视时钟</center>
<br>

复制12点处的刻度，快捷键是选中 Hour Indicator 12 并按 ctrl + D。将其重命名为 Hour Indicator 6 并将其位置调整为（0，-4，-0.25），使其处于 6 点的位置。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hour-indicator-6-hierarchy.png"/>
<img src=".\images\game-objects-and-scripts\hour-indicator-6-scene.png"/>
</div>
<center>6点和12点处的刻度</center>
<br>

一共复制12个刻度，并将他们移动、旋转到合适的位置，成品如图所示。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\all-hour-indicators.png"/>
</div>
<center>所有刻度</center>
<br>

### 2.5 创建指针
下一步是创建时钟的指针，我们从时针开始。

再复制一个刻度，并将其命名为 Hours Arm。然后创建一个 Clock Arm 材质挂在时针上。我们将时针的颜色设置为纯黑色，十六进制是 #000000。将时针缩放值的 X 刻度减小到 0.3， Y 刻度增加到 2.5。然后将其位置的 Y 调成 0.75，使其指向 12 点。此时时针在向相反的方向也留了一点长度，使时针在旋转时看起来好像有一点配重。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hours-arm-scene.png"/>
<img src=".\images\game-objects-and-scripts\hours-arm-inspector.png"/>
</div>
<center>时针</center>
<br>

时针应该是要围绕时钟表盘的中心旋转的，但是我们让其绕着 Z 轴旋转时，会发现它是围绕自己的中心旋转的。

<br>
<div align=center>
<video src=".\images\game-objects-and-scripts\CandidCleanEkaltadeta-mobile.mp4"/>
</div>
<center>时针围绕自身中心旋转</center>
<br>

发生这种现象是因为游戏对象的旋转都是相对于游戏对象的本地位置（Local Position）旋转的。要使它绕着表盘中心旋转，我们必须给它创建一个在表盘中心的枢轴（Pivot）对象作为其父节点，让时针绕着这个枢轴对象旋转。

创建一个新的空游戏对象并使其成为 Clock 的子对象，将其命名为 Hours Arm Pivot，并确保其位置（Position）和旋转（Rotation）均为零，并且其缩放值（Scale）均为 1，然后让时针成为枢轴的子节点。


<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hours-arm-hierarchy.png"/>
</div>
<center>带枢轴的时针</center>
<br>

现在我们旋转枢轴，会发现时针会被带动着绕枢轴旋转，也就绕着表盘正中心旋转了。确保你旋转的是枢轴，而不是时针本身。

<br>
<div align=center>
<video src=".\images\game-objects-and-scripts\DismalUnhealthyAzurevase-mobile.mp4"/>
</div>
<center>时针围绕自身中心旋转</center>
<br>

再复制两个时针用来创建分针和秒针。因为分针和秒针都需要绕着各自的枢轴旋转，所以复制要包括时针的枢轴，而不是仅仅复制时针。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\all-arms-hierarchy.png"/>
</div>
<center>时针、分针和秒针</center>
<br>

分针应该比时针更窄，更长，因此将其缩放调成（0.2， 4， 0.1）。同时将其位置的 Z 值改成 -0.35，因为分针应该在时针的上面。注意，这是调整分针本身，而不是其枢轴。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\minutes-arm-transform.png"/>
</div>
<center>分针的 Transform 组件</center>
<br>

同时调整秒针。缩放为（0.1，5，0.1），位置为（0，1.25，−0.45）。这些缩放、位置的数值都可以按照你的实际情况和喜好做出调整。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\seconds-arm-transform.png"/>
</div>
<center>秒针的 Transform 组件</center>
<br>

我们给秒针创建一个单独的材质，让它看起来更明显。我给了它一个深红色，十六进制是 #B30000。此外，由于完成了整个时钟的创建，我关闭了场景窗口中的网格。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\all-arms-scene.png"/>
</div>
<center>完整的时钟</center>
<br>

记得保存我们的场景，通过“文件/保存（File/Save）”来保存，也可以使用快捷键“Ctrl + S”。保存成功的话，层级窗口（Hierarchy）中场景名称旁边的星号会消失。

保持项目资源井井有条是一个好习惯。由于我们有三种材质，所以我们给他们新建一个文件夹，“资产/创建/文件夹（Assets/Create/Folder）”，命名为 Materials，并将材质拖进去。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\material-folder-one-column.png"/>
<img src=".\images\game-objects-and-scripts\material-folder-two-column.png"/>
</div>
<center>项目窗口中的材质文件夹，一列和两列布局</center>
<br>

## 3. 为时钟设置动画
我们的时钟目前不会显示时间，它只会一直停留在十二点。

要让它像一个真正的时钟那样工作，我们得给它添加自定义的行为。这需要我们为其添加自定义的组件，自定义组件通过脚本来定义。

### 3.1 C# 脚本
通过“资产/创建/C#脚本（Assets/Create/C# Script）”向项目添加新的脚本资产，并将其命名为 Clock。C# 是用于 Unity 脚本的编程语言，发音为 C-sharp。让我们也立即将其放在新的 Scripts 文件夹中，以保持项目整洁。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\script-asset-one-column.png"/>
<img src=".\images\game-objects-and-scripts\script-asset-two-column.png"/>
</div>
<center>脚本文件夹和 Clock 脚本</center>
<br>

选择脚本后，监视面板（Inspector）将显示其内容。但是想要编辑代码，我们必须使用代码编辑器。你可以通过监视面板中的“Open”按钮，或在项目窗口（Project）中双击脚本来打开脚本进行编辑。打开哪个编辑器可以通过 Unity 的首选项（Preferences）进行配置。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\clock-inspector.png"/>
</div>
<center>C# 资产的监视面板</center>
<br>

### 3.2 定义组件类型
在代码编辑器中加载脚本后，首先删除标准模板代码，因为我们将从头开始创建组件类型。

现在我们来定义时钟组件。我们要定义的不是组件的单个实例，相反，我们定义的是组件的类。一旦定义了一个类，我们就可以在 Unity 中创建多个属于这个类的组件，尽管我们在本教程中只创建一个组件。

在 C# 中，我们先定义一个 Clock 类。

```C#
public class Clock {}
```

我们的代码现在是有效的，但它还不是一个组件脚本。保存文件并切换回 Unity。Unity 编辑器将检测到脚本资源已更改，并触发重新编译。编译完成后，选中我们的脚本，在监视面板中 Unity 将通知我们该资产不包含 MonoBehaviour 脚本。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\non-component.png"/>
</div>
<center>非组件脚本</center>
<br>

这意味着我们不能使用此脚本在 Unity 中创建组件，因为要创建组件，我们的自定义的 Clock 组件类型必须扩展 Unity 的 MonoBehaviour 类型，继承其数据和功能。


<br>

> <center>Mono-Behaviour是上面意思？</center>
> 
> Unity 的设计思路是，我们通过对自定义的组件进行编程，以向游戏对象添加自定义行为。这就是 Behaviour 的意思。Mono 是指向 Unity 添加对自定义代码的支持的方式。它使用了 Mono 项目，该项目是 .NET 框架的多平台实现，是一个旧名称，由于向后兼容性，一直在被使用。

<br>

因此我们让 Clock 类继承 Unity 的 MonoBehaviour 类，该类在 UnityEngine 命名空间下。此时，Clock 类便可以用来创建组件了。

```C#
using UnityEngine;

public class Clock : MonoBehaviour {}
```

要用一个脚本资产创建组件，可以通过将脚本资产拖到游戏对象的监视面板来实现，也可以使用游戏对象监视面板的“添加组件（Add Component）” 按钮来添加。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\clock-with-component.png"/>
</div>
<center>Clock 对象与我们的 Clock 组件</center>
<br>

### 3.3 在代码中获取指针对象
要旋转指针，需要改变其枢轴对象 Transform 组件中的 Rotation 值。因此我们需要获取到指针的枢轴对象的 Transform 组件。我们先从时针开始。

先声明一个 Transform 类型的字段 hoursPivot，用来存放时针枢轴的 Transform 组件。

```C#
using UnityEngine;

public class Clock : MonoBehaviour {
    Transform hoursPivot;
}
```

我们的类现在定义了一个字段，该字段可以保存对另一个对象的引用，其类型必须是 Transform。我们必须确保它包含对时针枢轴 Transform 组件的引用。

默认情况下，字段是私有的，这意味着它们只能由属于 Clock 的代码访问。但是该类不知道我们的 Unity 场景，因此没有直接的方法将字段与正确的对象相关联。我们可以通过将字段声明为可序列化（SerializeField）来更改它。这意味着当 Unity 保存场景时，该字段应该被包含在场景的数据中，这通过将所有数据放在一个序列中（序列化）并将其写入文件来实现。

将字段标记为可序列化是通过向其附加属性来完成的，在本例中为 SerializeField。它用方括号括起来，写在字段声明前面，通常把它放在上面一行，但也可以放在同一行上。

```C#
[SerializeField]
Transform hoursPivot;
```

<br>

> <center>能直接将该字段定义为 Public 吗？</center>
> 
> 可以，定义为 Public 的字段也能在 Unity 的监视面板中显示并赋值，但由于 hoursPivot 不需要其他 C# 代码来访问，因此更好的做法是将其定义为私有。而且，即使是要设为 Public，优先做法也是使用 Public 的方法或者属性，而不是直接将字段设为 Public。

<br>

一旦字段可序列化，Unity 将检测到这一点并将其显示在时钟游戏对象 Clock 组件的监视面板中。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hours-pivot-field.png"/>
</div>
<center>Hours Pivot 字段</center>
<br>

要将该字段赋值为时针枢轴的 Transform 组件，可以将 Hours Arm Pivot 对象从层级面板（Hierarchy）中拖到监视面板里 Hours Pivot 右边的方框中。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\hours-pivot-connected.png"/>
</div>
<center>Hours Pivot 字段赋值</center>
<br>

### 3.4 获取所有指针
我们对分针和秒针做同样的操作。

```C#
[SerializeField]
Transform hoursPivot;

[SerializeField]
Transform minutesPivot;

[SerializeField]
Transform secondsPivot;

// 为了方便也可以写在一行
// [SerializeField]
// Transform hoursPivot, minutesPivot, secondsPivot;
```

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\all-pivots-connected.png"/>
</div>
<center>所有指针</center>
<br>

### 3.5 Awake 函数
我们现在可以访问三个指针的 Transform 组件了。要对他们进行操作，需要在一些特定的 Unity 事件函数中进行。

```C#
public class Clock : MonoBehaviour {

	[SerializeField]
	Transform hoursPivot, minutesPivot, secondsPivot;

	Awake() {}
}
```

`Awake()` 函数会在组件被唤醒的时候被 Unity 调用。唤醒组件一般发生在播放模式下创建或者加载游戏对象成功之后，我们现在是在编辑模式，所以`Awake()`此时不会被调用。

### 3.6 通过代码旋转
要旋转指针的枢轴，我们需要更改 Transform 组件的 localRotation 属性。虽然 Transform 组件的旋转在监视面板中是用欧拉角（以每根轴旋转的度数）表示的，但在代码中，其旋转是用四元数定义的。

<br>

> <center>什么是四元数？</center>
> 
> 四元数是一种3D旋转的表示方式，虽然不如欧拉角直观和容易理解，但是却也有许多有点，如不会发生万向节死锁。

<br>

我们用 `Quaternion.Euler()`函数来通过欧拉角来创建四元数，并将其返回的四元数赋值给Tran form 的 localRotation 属性。

```C#
    // 让 hoursPivot 绕 Z 轴顺时针旋转 30° 
	hoursPivot.localRotation = Quaternion.Euler(0f, 0f, -30f);
```

<br>

> <center>Transform.rotation 和 Transform.localRotation 有什么区别？</center>
> 
> localRotation 属性表示相对于其父节点的旋转，也是你在其监视面板中看到的旋转。相反，rotation 属性表示世界空间中的最终旋转，同时考虑了整个对象层级。

<br>

现在在编辑器中进入播放模式。你可以通过按编辑器窗口顶部中心的播放按钮播放，也可以使用快捷键“Ctrl + P”。Unity 会将聚焦到游戏窗口，该窗口渲染场景中主摄像机看到的内容。时钟组件会被唤醒，并且调用`Awake()`方法，把时针设置到一点钟位置。

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\one-o-clock.png"/>
</div>
<center>时针指向一点钟</center>
<br>

如果摄像机未聚焦在时钟上，你可以移动它以使时钟可见，但请记住，退出播放模式时场景会重置，因此在播放模式下对场景所做的任何更改都不会保留。但是，对于资产来说并非如此，对它们的更改始终存在。你还可以在播放模式下打开场景窗口，甚至打开多个场景和游戏窗口。在继续之前退出播放模式。

### 3.7 获取当前时间
下一步是弄清楚我们唤醒 Clock 时的当前时间。我们可以使用 `DateTime` 结构来访问正在运行的设备的系统时间。DateTime 不是 Unity 里的类型，它位于命名空间 Systm 中，是 .NET 框架核心功能的一部分，Unity 使用它获取系统时间。

`DateTime` 具有一个属性 `Now` ，该属性生成包含当前系统日期和时间的值。为了检查它是否正确，我们将在 Awake 开头将其打印到控制台。我们可以使用 `Debug.Log()` 方法来打印信息。

```C#
using System;
using UnityEngine;

public class Clock : MonoBehaviour {

	[SerializeField]
	Transform hoursPivot, minutesPivot, secondsPivot;

	void Awake () {
		Debug.Log(DateTime.Now);
		hoursPivot.localRotation = Quaternion.Euler(0f, 0f, -30f);
	}
}
```

现在，每次进入播放模式时，我们都会打印当前时间。你可以在控制台窗口和编辑器窗口底部的状态栏中看到它。

### 3.8 旋转指针
我们的时钟就要能工作了。

`DateTime.Now` 有一个属性 `Hour` 可以获取当前的小时数，要让时针显示当前小时，我们得将当前小时数乘以 −30° 旋转。

```C#
    hoursPivot.localRotation = Quaternion.Euler(0f, 0f, -30f * DateTime.Now.Hour);
```

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\four-o-clock.png"/>
</div>
<center>当前时间为四点</center>
<br>

为了明确 `-30` 这一数字的意义是从小时转换为度，我们可以定义一个转换因子的常量字段 `hoursToDegrees`。`Quaternion.Euler()` 的角度定义为浮点值，因此我们将转换因子声明为 float 型。然后在 `Awake()` 钟乘以该字段。

```C#
    const float hoursToDegrees = -30f;

	[SerializeField]
	Transform hoursPivot, minutesPivot, secondsPivot;

	void Awake () {
		hoursPivot.localRotation =
			Quaternion.Euler(0f, 0f, hoursToDegrees * DateTime.Now.Hour);
	}
```

然后为分针和秒针都进行相同的处理，对于分针和秒针，转换因子是 -6°。

```C#
    const float hoursToDegrees = -30f, minutesToDegrees = -6f, secondsToDegrees = -6f;

	[SerializeField]
	Transform hoursPivot, minutesPivot, secondsPivot;

	void Awake () {
		hoursPivot.localRotation =
			Quaternion.Euler(0f, 0f, hoursToDegrees * DateTime.Now.Hour);
		minutesPivot.localRotation =
			Quaternion.Euler(0f, 0f, minutesToDegrees * DateTime.Now.Minute);
		secondsPivot.localRotation =
			Quaternion.Euler(0f, 0f, secondsToDegrees * DateTime.Now.Second);
	}
```

<br>
<div align=center>
<img src=".\images\game-objects-and-scripts\time-5-16-31.png"/>
</div>
<center>当前时间为5：16：31</center>
<br>

我们访问了三次 `DateTime.Now` 属性。每次我们访问该属性时，都需要一些开销和时间，所以理论上三次调用可能会得到三个不同的时间值。为了确保这种情况不会发生，我们应该只访问一次时间。所以我们在方法中声明一个变量并为其赋上当前的时间值，然后使用此值。

```C#
void Awake () {
		DateTime time = DateTime.Now;
		hoursPivot.localRotation =
			Quaternion.Euler(0f, 0f, hoursToDegrees * time.Hour);
		minutesPivot.localRotation =
			Quaternion.Euler(0f, 0f, minutesToDegrees * time.Minute);
		secondsPivot.localRotation =
			Quaternion.Euler(0f, 0f, secondsToDegrees * time.Second);
	}
```

### 3.9 让指针动起来
进入播放模式时，我们得到当前时间，但之后时钟保持静止。要使时钟与当前时间保持同步，需要将 Awake 方法的名称更改为 Update。这是另一个特殊的事件方法，只要我们保持播放模式，Unity 就会每帧调用一次，而不仅仅是一次。

```C#
void Update () {
		DateTime time = DateTime.Now;
		hoursPivot.localRotation =
			Quaternion.Euler(0f, 0f, hoursToDegrees * time.Hour);
		minutesPivot.localRotation =
			Quaternion.Euler(0f, 0f, minutesToDegrees * time.Minute);
		secondsPivot.localRotation =
			Quaternion.Euler(0f, 0f, secondsToDegrees * time.Second);
	}
```

<br>
<div align=center>
<video src=".\images\game-objects-and-scripts\RelievedFamiliarHamadryad-mobile.mp4"/>
</div>
<center>更新时间</center>
<br>

<br>

> <center>什么是帧？</center>
> 
> 在播放模式下，Unity 会从主摄像机的视角持续渲染场景。渲染完成后，将渲染结果给显示器显示，然后显示器将显示该帧，直到它获得下一帧。在渲染新帧之前，所有内容都会更新。因此，Unity 会经历一系列更新、渲染、更新、渲染等。一次的“更新、渲染”通常被视为一帧，尽管实际上会更复杂。

<br>

请注意，当添加 `Update()` 函数后，我们的组件在监视面板中的名称前面出现一个复选框,这个复选框可以让我们禁用该组件，从而阻止 Unity 调用其 `Update()` 方法(Awake 依然会被调用)。

### 3.10 连续旋转
时钟的指针准确显示了当前的小时、分钟和秒，但它的行为就像一个数字时钟，离散地运行，不够丝滑。通常，时钟具有缓慢旋转的指针，来模拟连续流逝的时间。

`DateTime` 不包含小数数据，但幸运的是，它确实有一个属性 TimeOfDay 为我们提供了一个值 TimeSpan，该值的 TotalHours、TotalMinutes 和 TotalSeconds 属性有我们需要的足够精度的时间小数值。它们是 double 型，因此需要强制转换为 float 型。

```C#
void Update () {
	TimeSpan time = DateTime.Now.TimeOfDay;
	hoursPivot.localRotation =
		Quaternion.Euler(0f, 0f, hoursToDegrees * time.TotalHours);
	minutesPivot.localRotation =
		Quaternion.Euler(0f, 0f, minutesToDegrees * time.TotalMinutes);
	secondsPivot.localRotation =
		Quaternion.Euler(0f, 0f, secondsToDegrees * time.TotalSeconds);
}
```

现在，你已经了解了在 Unity 中创建对象和编写代码的基础知识。下一个教程是构建图形。