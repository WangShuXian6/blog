#### Package Diagram  包图
>要创建包图：

>首先选择一个元素，其中包含一个新的Package Diagram作为子元素。

>选择型号| 添加图| 菜单栏中的包装图或选择添加图| 上下文菜单中的包图。

>也可以看看

>UML包图 

>https://www.uml-diagrams.org/package-diagrams-overview.html

***
#### Package 包

>要在图表上创建包：

>在工具箱中选择包。

>在图上拖动为Package的大小。

>要通过Menu创建一个Package（仅限模型元素）：

>选择要包含新包的元素。

>选择型号| 添加| 菜单栏中的包或添加| 上下文菜单中的包。

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add Note : Add a linked note. 添加链接的备注

>Add Sub-Package : Add a sub-package. 添加子包

>Add Dependant Package : Add a dependant package. 添加依赖包


>Add Depending Package : Add a depending package. 添加依赖包

***
#### Model 模型
>要在图表上创建模型：

>在工具箱中选择模型。

>在图上拖动为Model的大小。

>要通过Menu创建模型（仅限模型元素）：

>选择要包含新模型的元素。

>选择型号| 添加| 菜单栏中的模型或添加| 上下文菜单中的模型。

***
#### Subsystem 子系统
>要在图表上创建子系统：

>在工具箱中选择子系统。

>在图表上拖动作为子系统的大小。

>要通过Menu创建子系统（仅限模型元素）：

>选择要包含新子系统的元素。

>选择型号| 添加| 菜单栏中的子系统或添加| 上下文菜单中的子系统。


```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 更改可见性属性

>Add Note : Add a linked note. 添加链接的备注

>Add Provided Interface : Add a provided interface. 添加提供的接口

>Add Required Interface : Add a required interface. 添加所需的接口

***
#### Containment 
>要建立遏制关系：

>选择遏制在工具箱。

>从元素（要包含）拖动并放在容器元素上。

>注意

>没有Containment模型元素。Containment视图元素仅显示两个元素之间的包含关系。（包含的元素在模型资源管理器中显示为子元素）

***
#### 
>包图是UML 结构图 ，显示了包级别设计系统的结构 。以下元素通常在包图中绘制： 包，可 包装元素， 依赖项， 元素导入， 包导入， 包合并。

>模型图 是UML辅助 结构图 ，它显示了系统的一些抽象或特定视图，用于描述系统的体系结构，逻辑或行为方面。例如，它可以显示多层（也称为多层）应用程序的架构 - 多层应用程序模型。

#### 包装图
>包装图的一些主要元素如下图所示。网上购物，移动购物，手机购物和邮件购物 包 合并 购物车包。相同的4个包 使用 Payment包。付款和购物车包都 导入 其他包。

>UML包图元素 - 包，导入，访问，使用，合并。

![image](https://user-images.githubusercontent.com/30850497/60442102-e9785080-9c4a-11e9-8b02-6a9f74105107.png)

#### 模型图
>模型图是UML辅助 结构图 ，它显示了系统的一些抽象或特定视图，用于描述系统的一些架构，逻辑或行为方面。

>下图显示了模型图的一些主要元素。分层应用程序是一个 “容器”模型 ，它包含三个其他 模型 - 表示层，业务层和数据层。有 依赖 这些包含模型之间定义。

>UML模型图元素 - 模型，包，依赖

![image](https://user-images.githubusercontent.com/30850497/60442148-ff861100-9c4a-11e9-9c7d-a72281f6238f.png)

>模型通常包含 包。包可以具有 依赖关系或其他关系，例如 导入，在它们之间定义。

>您可以在此处找到一些 包图示例：
