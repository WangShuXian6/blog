#### Class Diagram 类图
>Select Model | Add Diagram | Class Diagram in the Menu Bar or select Add Diagram | Class Diagram in Context Menu.

>创建类图

>首先选择一个元素，其中包含一个新的类图作为子元素。

>选择型号| 添加图| 菜单栏中的类图或选择添加图| 上下文菜单中的类图。

>http://www.uml-diagrams.org/class-diagrams-overview.html

***
#### Model Element 模型元素
>模型元素是所有UML模型元素的抽象元素。

>通过双击或按下选定的模型元素，可以将QuickEdit用于Enter模型元素。

***
#### Classifier 分类器
>分类器是以下的抽象元素：

>Class 类

>Interface 接口

>Signal 信号

>DataType 数据类型

>PrimitiveType

>Enumeration 列举

>Component 零件

>Artifact 神器

>可以通过双击或按选定的分类器将QuickEdit用于分类Enter器

>Name Expression : Edit name expression. 名称表达式：编辑名称表达式。 名称表达的语法

>Syntax of Name Expression

>expression ::= [ '<<' stereotype `>>` ] [ visibility ] name

>stereotype ::= (identifier)

>visibility ::= '+' | '#' | '-' | '~'

>name ::= (identifier)

>Visibility : Change visibility property. 可见性：更改可见性属性。

>Add Note : Add a linked note. 添加注意：添加链接的备注。

>Add Attribute (Ctrl+Enter) : Add an attribute. 添加属性

>Add Operation (Ctrl+Shift+Enter) : Add an operation. 添加操作

>Add Reception : Add a reception. 添加接待处
***
#### Class 类
>要创建一个类：

>在工具箱中选择类。

>在图上拖动为Class的大小。

>要通过Menu创建一个Class（仅限模型元素）：

>选择要包含新类的元素。

>选择型号| 添加| 菜单栏中的类或添加| 上下文菜单中的类。

>您可以通过双击或按选定的类来使用QuickEdit for EnterClass。

>Name Expression : Edit name expression. 名称表达式：编辑名称表达式。 名称表达的语法

>Syntax of Name Expression

>expression ::= [ '<<' stereotype `>>` ] [ visibility ] name

>stereotype ::= (identifier)

>visibility ::= '+' | '#' | '-' | '~'

>name ::= (identifier)

>Visibility : Change visibility property. 可见性：更改可见性属性

>Add Note : Add a linked note. 添加注意：添加链接的备注

>Add Attribute (Ctrl+Enter) : Add an attribute. 添加属性

>Add Operation (Ctrl+Shift+Enter) : Add an operation. 添加操作

>Add Template Parameter : Add a template parameter.  添加模板参数

>Add Reception : Add a reception. 添加接待处

>Add Sub-Class : Add a sub-class. 添加子类

>Add Super-Class : Add a super class. 添加超类

>Add Provided Interface : Add a provided interface. 添加提供的接口

>Add Required Interface : Add a required interface. 添加必需的接口

>Add Associated Class : Add an associated class. 添加关联类

>Add Aggregated Class : Add an aggregated class. 添加聚合类

>Add Composited Class : Add a composited class. 添加合成类

>Add Port : Add a port. 添加端口

>Add Part : Add a part. 添加零件

***
#### Attribute 属性
>要添加属性：

>选择一个分类器。

>选择型号| 添加| 菜单栏中的属性或添加| 上下文菜单中的属性。

>您可以通过双击或按下所选属性来使用QuickEdit for EnterAttribute。

>Attribute Expression : Edit Attribute expression. 属性表达式：编辑属性表达式。

>Syntax of Attribute Expression  属性表达式的语法

```
attribute ::= [ '<<' stereotype `>>` ] [ visibility ] name [':' type ] [ '[' multiplicity ']' ] [ '=' defaut-value ]
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
type ::= (identifier)
multiplicity ::= multiplicity-bound [ '..' multiplicity-bound ]
multiplicity-bound ::= (number) | '*'
default-value ::= (string)
```
>Visibility : Change visibility property. 可见性：更改可见性属性

>Add (Ctrl+Enter) : Add one more attribute in the below. 在下面添加一个属性

>Delete (Ctrl+Delete) : Delete the attribute 删除属性

>Move Up (Ctrl+Up) : Move the attribute up. 向上移动属性

>Move Down (Ctrl+Down) : Move the attribute down. 向下移动属性

***
#### Operation 操作

```
operation ::= [ '<<' stereotype `>>` ] [ visibility ] name [ '(' parameter-list ')' ] [ ':' return-type ]
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
parameter-list ::= parameter [ ',' parameter ]*
parameter ::= (identifier)
return-type ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add (Ctrl+Enter) : Add one more operation in the below. 在下面添加一个操作

>Delete (Ctrl+Delete) : Delete the operation 删除操作

>Move Up (Ctrl+Up) : Move the operation up. 向上移动操作

>Move Down (Ctrl+Down) : Move the operation down. 向下移动操作

***
#### Parameter 参数
***
#### Template Parameter 模板参数
>模板参数表达式
```
template-parameter ::= [ '<<' stereotype `>>` ] [ visibility ] name [':' type ] [ '=' defaut-value ]
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
type ::= (identifier)
default-value ::= (string)
```
>Visibility : Change visibility property. 更改可见性属性

>Add (Ctrl+Enter) : Add one more template parameter in the below. 在下面添加一个模板参数

>Delete (Ctrl+Delete) : Delete the template parameter. 删除模板参数

>Move Up (Ctrl+Up) : Move the template parameter up. 向上移动模板参数

>Move Down (Ctrl+Down) : Move the template parameter down. 向下移动模板参数

***
#### Template Parameter Substitution 模板参数替换
>要添加模板参数替换：

>选择模板绑定元素。

>选择型号| 添加| 菜单栏中的模板参数替换或添加| 上下文菜单中的模板参数替换。

>每个模板参数替换都应具有formal分配给模板元素的模板参数的actual属性和分配给模板参数的实际值的属性。

***
#### Interface 接口
>要创建接口：

>在工具箱中选择接口。

>在图上拖动为Interface的大小。

>要通过Menu创建接口（仅限模型元素）：

>选择要包含新接口的元素。

>选择型号| 添加| 菜单栏中的界面或添加| 上下文菜单中的界面。

>名称表达的语法

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add Note : Add a linked note. 添加链接的备注

>Add Attribute (Ctrl+Enter) : Add an attribute. 添加属性

>Add Operation (Ctrl+Shift+Enter) : Add an operation. 添加操作

>Add Reception : Add a reception. 添加接待处

>Add Sub-Interface : Add a sub-interface. 添加子接口

>Add Super-Interface : Add a super interface. 添加超级接口

>Add Realizing Class : Add an realizing class. 添加实现类

>要将接口显示为套接字表示法，接口应具有依赖项

***
#### Signal 信号
> 创建信号：

> 在工具箱中选择信号。

> 在图上拖动为Signal的大小。

> 要通过Menu创建Signal（仅限模型元素）：

> 选择要包含新信号的元素。

> 选择型号| 添加| 菜单栏中的信号或添加| 上下文菜单中的信号。

***
#### DataType 数据类型
>要创建DataType：

>在工具箱中选择DataType。

>在图上拖动为DataType的大小。

>要通过Menu创建DataType（仅限模型元素）：

>选择要包含新DataType的元素。

>选择型号| 添加| 菜单栏中的DataType或添加| 上下文菜单中的DataType。

***
#### PrimitiveType

***
#### Enumeration 枚举
```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add Note : Add a linked note. 添加链接的备注

>Add Literal (Ctrl+Enter) : Add an enumeration literal. 添加枚举文字

>Add Operation (Ctrl+Shift+Enter) : Add an operation. 添加操作

***
#### Enumeration Literal  枚举文字
```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add (Ctrl+Enter) : Add one more literal in the below. 在下面添加一个文字

>Delete (Ctrl+Delete) : Delete the literal 删除文字

>Move Up (Ctrl+Up) : Move the literal up. 向上移动文字

>Move Down (Ctrl+Down) : Move the literal down. 向下移动文字
***
#### Relationship 关系
>关系是表示UML元素之间关系的抽象元素

>关系的子类是

>Generalization 概括

>Association 

>Aggregation 聚合

>Composition 组成

>Dependency 依赖

>Interface Realization 接口实现

>Component Realization 组件实现

>Include 包括

>Exclude 排除

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add Note : Add a linked note. 添加链接的备注

***
#### Generalization 泛化
>要创建泛化：

>在工具箱中选择“ 泛化”。

>从元素拖动（特殊）并放在另一个元素上（一般）。

***
#### Association 关联
>要创建关联（或定向关联）：

>在工具箱中选择关联（或定向关联）。

>从元素拖动并放到另一个元素上。

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Navigability : Change navigability property. 更改导航属性

>Aggregation Kind : Change aggregationKind property. 聚合种类

>Multiplicity : Change multiplicity property. 更改多重性属性

>Add Qualifier : Add a qualifier (attribute) to the AssociationEnd. 添加限定符

***
#### Aggregation 聚合
>要创建聚合：

>在工具箱中选择聚合。

>从元素拖动（成为一部分）并放在另一个元素上（作为整体）。

>您还可以通过在关联的末端双击来使用QuickEdit for AssociationEnd
***
#### Composition 组合
>要创建组合：

>在工具箱中选择组合。

>从元素拖动（成为一部分）并放在另一个元素上（作为整体）

>您还可以通过在关联的末端双击来使用QuickEdit for AssociationEnd（

***
#### Dependency 依赖
>要创建依赖关系：

>在“ 工具箱”中选择“ 依赖关系”。

>从元素（客户端）拖动并放在另一个元素（供应商）上。

***
#### Interface Realization  接口实现
>要创建接口实现：

>在Toolbox中选择Interface Realization。

>从元素中拖出（实现）并放在界面上（待实现）。

***
#### Association Class 关联类
>通过链接两个分类器来创建关联类：

>在工具箱中选择关联类。

>从元素拖动并放下另一个元素。

>将创建与该关联相关联的关联和类。

>通过链接关联和类来创建关联类：

>在工具箱中选择关联类。

>从关联（或类）拖动并放在类（或关联）上。

>该课程将与协会相关联。

***
#### Template Binding  模板绑定
>要创建模板绑定：

>在工具箱中选择模板绑定。

>从绑定元素拖动并放置模板元素。

>如果需要，请创建模板参数替换

>Visibility : Change visibility property. 更改可见性属性

>Add Note : Add a linked note. 添加链接的备注

>Add Template Parameter : Add a template parameter substitution 添加模板参数替换

***
#### Frame 框架
>要创建特定模型元素的框架视图：

>在工具箱中选择框架。

>在图上拖动为Frame的大小。

>在Element Picker对话框中选择 Frame表示的模型元素

***
### UML类和对象图概述
>https://www.uml-diagrams.org/class-diagrams-overview.html

>类图是UML 结构图 ，它显示了类和 接口级别的设计系统的结构 ，显示了它们的特性， 约束 和关系 - 关联， 概括， 依赖等。

>一些常见的类图类型是：

>域模型图，

>实现类图。

>对象图 可以被视为实例级类图，它显示 了类和接口（对象）的实例规范， 具有值规范的插槽和 链接 （关联实例）。

***
#### 领域模型图
>域图概述 - 类，接口，关联，用法，实现，多样性。

![image](https://user-images.githubusercontent.com/30850497/60442436-9a7eeb00-9c4b-11e9-828f-96a46fa61c3e.png)

#### 实现类图
>实现类图的元素 - 类，接口，关联，用法，实现。

![image](https://user-images.githubusercontent.com/30850497/60442476-aec2e800-9c4b-11e9-87c8-79e38cf79f1b.png)

#### 对象图
>对象图在现在过时的UML 1.4.2规范中定义 为 “实例图，包括对象和数据值。静态对象图是类图的实例;它显示了系统详细状态的快照。时间点。” 它还声明对象图是 “带有对象且没有类的类图”。

>UML 2.4规范不提供对象图的定义， 除了 “以下节点和边缘通常在对象图中绘制：实例规范和链接（即关联）”。

>注意，UML 2.5标准层次结构图（参见 UML 2.5图表概述），显示类图和对象图完全无关。其他一些权威的UML源声明， 仅包含 实例规范的组件图 和部署图 也是特殊类型的对象图。

>对象图下面概述显示对象图的一些主要元件-命名和匿名 实例规格 为对象， 槽 具有值的规格，和 链接 （实例关联）。

>对象图概述 - 实例规范，值规范，插槽和链接

![image](https://user-images.githubusercontent.com/30850497/60442528-ce5a1080-9c4b-11e9-9191-a0385e97fcc0.png)
