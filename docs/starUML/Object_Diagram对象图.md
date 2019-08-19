#### Object Diagram 对象图
>要创建对象图：

>首先选择一个元素，其中包含一个新的Object Diagram作为子元素。

>选择型号| 添加图| 菜单栏中的对象图或选择添加图| 上下文菜单中的对象图。

>UML对象图 http://www.uml-diagrams.org/class-diagrams-overview.html#object-diagram

***
#### Object 对象
>要创建对象：

>在工具箱中选择对象。

>在图上拖动为Object的大小。

>您可以通过双击或按下所选对象来使用QuickEdit for EnterObject。

>名称表达的语法

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 可见性：更改可见性属性。

>Add Note : Add a linked note. 添加注意：添加链接的备注。

>Add Slot (Ctrl+Enter) : Add a slot. 添加插槽（Ctrl+Enter）：添加插槽。

>Add Linked Object : Add a linked object. 添加链接对象

***
#### Slot 插槽
>添加插槽：

>选择一个实例。

>选择型号| 添加| 菜单栏中的插槽或添加| 上下文菜单中的插槽。

>您可以通过双击或按下选定的插槽来使用QuickEdit for EnterSlot。

>Slot表达式的语法

```
slot ::= [ '<<' stereotype `>>` ] [ visibility ] name [':' type ] [ '=' value ]
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
type ::= (identifier)
value ::= (string)
```
>Visibility : Change visibility property. 可见性：更改可见性属性。

>Add (Ctrl+Enter) : Add one more slot in the below. 添加（Ctrl+Enter）：在下面添加一个插槽。

>Delete (Ctrl+Delete) : Delete the slot 删除（Ctrl+Delete）：删除插槽

>Move Up (Ctrl+Up) : Move the slot up.  上移（Ctrl+Up）：向上移动插槽。

>Move Down (Ctrl+Down) : Move the slot down. 下移（Ctrl+Down）：向下移动插槽。

***
#### Artifact Instance 工件实例
>要创建工件实例：

>在“ 工具箱”中选择“ 工件实例”。

>在图表上拖动为工件实例的大小。

>您可以将QuickEdit用于模型元素（请参阅模型元素）。

***
#### Component Instance 组件实例
>要创建组件实例：

>在工具箱中选择组件实例。

>在图表上拖动为组件实例的大小。

>您可以将QuickEdit用于模型元素（请参阅模型元素）。

***
#### Node Instance 节点实例
>要创建节点实例：

>在“ 工具箱”中选择“ 节点实例”。

>在图上拖动作为Node Instance的大小。

>您可以将QuickEdit用于模型元素（请参阅模型元素）。

***
#### Link 链接
>要创建链接（或定向链接）：

>在工具箱中选择链接（或定向链接）。

>从实例拖动并放在另一个实例上。

>您可以使用QuickEdit进行关系（请参阅关系）。

>您还可以通过双击链接的末尾侧面来使用QuickEdit for LinkEnd。

>名称表达的语法

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 可见性：更改可见性属性。

>Navigability : Change navigability property. 导航：更改导航属性。

>Aggregation Kind : Change aggregationKind property. 聚合种类：更改aggregationKind属性。

>Multiplicity : Change multiplicity property. 多重性：更改多重性属性。

***
#### 对象图
>对象图在现在过时的UML 1.4.2规范中定义 为 “实例图，包括对象和数据值。静态对象图是类图的实例;它显示了系统详细状态的快照。时间点。” 它还声明对象图是 “带有对象且没有类的类图”。

>UML 2.4规范不提供对象图的定义， 除了 “以下节点和边缘通常在对象图中绘制：实例规范和链接（即关联）”。

>注意，UML 2.5标准层次结构图（参见 UML 2.5图表概述），显示类图和对象图完全无关。其他一些权威的UML源声明， 仅包含 实例规范的组件图 和部署图 也是特殊类型的对象图。

>对象图下面概述显示对象图的一些主要元件-命名和匿名 实例规格 为对象， 槽 具有值的规格，和 链接 （实例关联）。

>对象图概述 - 实例规范，值规范，插槽和链接

![image](https://user-images.githubusercontent.com/30850497/60442528-ce5a1080-9c4b-11e9-9191-a0385e97fcc0.png)
