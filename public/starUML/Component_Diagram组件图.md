#### Component Diagram 组件图
>要创建组件图：

>首先选择一个元素，其中包含一个新的组件图作为子元素。

>选择型号| 添加图| 菜单栏中的组件图或选择添加图| 上下文菜单中的组件图。

>UML组件图 http://www.uml-diagrams.org/component-diagrams.html

***
#### Component 组件
>要创建组件：

>在工具箱中选择组件。

>在图表上拖动为Component的大小。

>要通过Menu创建Component（仅限模型元素）：

>选择要包含新Component的Element。

>选择型号| 添加| 菜单栏中的组件或添加| 上下文菜单中的组件。

>您可以通过双击或按下所选组件来使用QuickEdit for EnterComponent。

>名称表达的语法

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 可见性：更改可见性属性。

>Add Note : Add a linked note. 添加注意：添加链接的备注。

>Add Attribute (Ctrl+Enter) : Add an attribute. 添加属性（Ctrl+Enter）：添加属性。

>Add Operation (Ctrl+Shift+Enter) : Add an operation. 添加操作（Ctrl+Shift+Enter）：添加操作。

>Add Reception : Add a reception. 添加接待处

>Add Provided Interface : Add a provided interface.  添加提供的接口

>Add Required Interface : Add a required interface. 添加必需的接口

>Add Port : Add a port. 添加端口

>Add Part : Add a part. 添加零件

>要抑制属性，请参阅抑制属性。https://github.com/staruml/staruml-gitbook/blob/master/formatting-diagram.md#suppress-attributes

>要禁止操作，请参阅抑制操作。 https://github.com/staruml/staruml-gitbook/blob/master/formatting-diagram.md#suppress-operations

>要显示或隐藏操作签名，请参阅显示操作签名。 https://github.com/staruml/staruml-gitbook/blob/master/formatting-diagram.md#show-operation-signature

***
#### Artifact 工件
>要创建一个工件：

>在工具箱中选择工件。

>在图表上拖动为Artifact的大小。

>要通过Menu创建工件（仅限模型元素）：

>选择要包含新工件的元素。

>选择型号| 添加| 菜单栏中的工件或添加| 上下文菜单中的工件。

>您可以将QuickEdit用于分类器（请参阅分类器）。

>要抑制属性，请参阅抑制属性。

>要禁止操作，请参阅抑制操作。

>要显示或隐藏操作签名，请参阅显示操作签名。

***
#### Component Realization 组件实现
>要创建组件实现：

>在工具箱中选择组件实现。

>从一个元素拖动（实现）并放下一个Component（即可实现）。

>您可以使用QuickEdit进行关系（请参阅关系）。

***
#### UML Component Diagrams UML组件图
>组件图显示了组件，提供的和所需的接口，端口以及它们之间的关系。此类图表用于基于组件的开发（CBD），以描述具有面向服务的体系结构（SOA）的系统。

>基于组件的开发基于以下假设：如果需要，可以重复使用先前构建的组件，并且可以用其他“等效”或“符合”组件替换组件。

>实现组件的工件旨在能够独立部署和重新部署，例如更新现有系统。

>UML中的组件可以表示

>逻辑组件（例如，业务组件，流程组件）和

>物理组件（例如，CORBA组件，EJB组件，COM +和.NET组件，WSDL组件等），
以及实现它们的工件以及部署和执行它们的节点。预计将针对特定组件技术以及相关的硬件和软件环境开发基于组件的配置文件。

>以下节点和边通常在组件图中绘制： 组件， 接口， 提供的接口， 所需的接口， 类， 端口， 连接器， 工件， 组件实现， 依赖性， 用法。这些主要元素如下图所示。

>UML组件图的主要元素 - 组件，提供的接口，所需的接口，端口，连接器。

![image](https://user-images.githubusercontent.com/30850497/60488024-6e0eb180-9cd3-11e9-808c-145a7d52c3ae.png)

>零件 https://www.uml-diagrams.org/component.html

![image](https://user-images.githubusercontent.com/30850497/60488067-90083400-9cd3-11e9-83ee-5d9255efd01c.png)