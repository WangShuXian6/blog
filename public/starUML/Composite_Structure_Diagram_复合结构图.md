#### Composite Structure Diagram 复合结构图
>要创建复合结构图：

>首先选择一个元素，其中包含一个新的复合结构图作为子元素。

>选择型号| 添加图| 菜单栏中的复合结构图或选择添加图| 上下文菜单中的复合结构图。

>http://www.uml-diagrams.org/composite-structure-diagrams.html

***
#### Collaboration 合作
>要创建协作：

>在工具箱中选择协作。

>在图表上拖动为Collaboration的大小。

>要通过Menu创建Collaboration（仅限模型元素）：

>选择要包含新协作的元素。

>选择型号| 添加| 菜单栏中的协作或添加| 上下文菜单中的协作。

>您可以将QuickEdit用于模型元素（请参阅模型元素）。

***
#### Port 端口
>要创建端口：

>在工具箱中选择端口。

>单击要包含端口的元素（例如Class）。

>要通过Menu创建端口（仅限模型元素）：

>选择要包含新端口的元素。

>选择型号| 添加| 菜单栏中的端口或添加| 上下文菜单中的端口。

>您可以通过双击或按下所选端口来使用QuickEditEnter端口。

>名称表达的语法

```
expression ::= [ '<<' stereotype `>>` ] [ visibility ] name
stereotype ::= (identifier)
visibility ::= '+' | '#' | '-' | '~'
name ::= (identifier)
```
>Visibility : Change visibility property. 可见性：更改可见性属性

>Add Note : Add a linked note. 添加注意：添加链接的备注

>Select Type : Select a Classifier and assign it to type property. 选择类型：选择分类器并将其分配给类型属性

>Create Type : Create a Class and assign it to type property. 创建类型：创建一个类并将其分配给类型属性

>Add Provided Interface : Add a provided interface. 添加提供的接口：添加提供的接口

>Add Required Interface : Add a required interface. 添加必需的接口：添加所需的接口

>Add Connected Part : Add a connected part. 添加连接部件

***
#### Part 零件
>要创建零件：

>在工具箱中选择零件。

>单击要包含Part的元素（例如Class）。

>您可以将QuickEdit用于端口（请参阅端口）。

>注意

>实际上，Part相当于Attribute，但在图表上表示不同。

***
#### Connector 连接器
>要创建连接器：

>在工具箱中选择连接器。

>从元素（例如Port）拖动并放在另一个元素（例如Part）上。

>您可以使用QuickEdit进行关系（请参阅关系）

***
#### Collaboration Use 
>要创建协作使用：

>选择协作使用的工具箱。

>在图表上拖动协作使用的大小。

>您可以将QuickEdit用于模型元素（请参阅模型元素）。

***
#### Role Binding 角色绑定
>要创建角色绑定：

>在工具箱中选择角色绑定。

>从协作中拖动使用并放置元素（例如，部分）。

>您可以使用QuickEdit进行关系（请参阅关系）。

***
#### UML复合结构图
>复合结构图可用于显示：

>分类器的内部结构 - 内部结构图，

>分类器通过端口与环境的交互 ，

>协作行为 - 协作使用图。

>此类图表的术语“结构”在UML中定义为互连元素的组合，表示通过通信链接协作的运行时实例，以实现一些共同目标。

***
#### 内部结构图
>内部结构图显示 了分类器的内部结构 - 将该分类器分解为其属性，部件和关系。

>以下图形元素通常在复合结构图中绘制，该 结构图显示分类器的内部结构： 类， 部件， 端口， 连接器， 用法。

>复合结构图概述显示结构化分类器的内部结构元素 - 角色，部件，连接器。

![image](https://user-images.githubusercontent.com/30850497/60477139-d5673a00-9cb0-11e9-937a-ac93d13aa3bd.png)

>内部结构图的一些 示例：

>内部结构 - 银行ATM复合结构 https://www.uml-diagrams.org/bank-atm-uml-composite-diagram-example.html?context=cst-examples

>内部结构 - Tomcat 7服务器组合结构 https://www.uml-diagrams.org/tomcat-server-uml-composite-structure-diagram-example.html?context=cst-examples

***
#### 协作使用图表
>系统 的行为是设计中的系统将实现的功能或已由某些现有系统实现的功能。系统中的对象通常彼此协作以产生系统的行为。

>协作 的行为最终将由一组协作实例（由分类器指定）展示，这些实例通过发送信号或调用操作彼此通信。但是，为了理解设计中使用的机制，仅描述这些分类器的那些方面以及它们与完成从这些分类器投射的任务或相关任务集所涉及的交互可能是重要的。

>协作允许我们通过识别实例将扮演的特定角色来仅描述一组实例的合作的相关方面。

>接口允许指定实例的外部可观察属性，而无需确定最终将用于指定此实例的分类器。因此，协作中的角色通常会通过接口键入，然后规定参与实例必须展示的属性，但不会确定哪个类将指定参与的实例。

>以下节点和边缘通常在复合结构图中绘制，该 结构图显示协作的行为： 协作， 连接器， 部件， 协作专业化， 依赖性。

>协作元素 - 角色，部件，连接器。

>协作访问显示医生和患者角色的合作。

![image](https://user-images.githubusercontent.com/30850497/60477221-195a3f00-9cb1-11e9-9509-c4c437ce6289.png)

>协作使用表示一个特定用途（发生由所描述的图案的）或应用程序的协作 涉及特定类或实例播放一个具体情况角色协作的。协作使用通过将来自该上下文的特定实体绑定到协作的角色，显示协作所描述的模式如何应用于给定的 上下文。

>协作使用元素 - 角色，部分，角色绑定。

>协作使用childVisit代表

>访问协作的一个特定用途。

![image](https://user-images.githubusercontent.com/30850497/60477246-3131c300-9cb1-11e9-9e53-685170df4aee.png)

>分类器（在内部结构和协作中）扩展，具有自己的 协作用途。这些协作使用链接 与分类器的协作来给出分类器行为的描述。

>可以选择分类器拥有的协作用途之一作为整体表示分类器的行为。通过该协作使用与分类器相关的协作示出了与该分类器的结构特征相对应的实例（例如，其属性和部分）如何交互以生成分类器的整体行为。

>的代表协作可以用于在不同的抽象级别比由所提供的提供的分类器的行为的描述 内部结构 的分类器。分类的属性映射到角色的协作 由角色绑定 中的协作使用。

>您可以 在此处查看协作图的 示例：

>https://www.uml-diagrams.org/observer-pattern-uml-composite-diagram-example.html

>协作用作设计模式 - 观察者模式

> Observer Pattern https://www.uml-diagrams.org/observer-pattern-uml-composite-diagram-example.html

***
#### 银行ATM UML复合结构图示例
>这是UML 内部结构图的一个例子， 它显示了银行自动柜员机（ATM）的复合结构。

>该图的目的是显示>银行ATM的内部结构以及ATM的不同部分之间的关系 。

>银行ATM通常由几个设备组成，如中央处理器单元（CPU），加密处理器，内存，客户显示器，功能键按钮（通常位于显示器附近），磁性和/或智能芯片读卡器，加密密码键盘，客户收据打印机，保险库，调制解调器。

>银行ATM内部结构UML图示例

![image](https://user-images.githubusercontent.com/30850497/60484909-b2974e80-9ccd-11e9-9cf8-dee67bf331f8.png)


>保险柜存储需要限制访问的设备和部件，包括现金分配机制，存款机制，多个安全传感器（例如磁，热，地震，气体），电子日志系统以维护系统日志等。自动提款机包括多个可移动的现金盒和存款机制 - 可拆卸的存储盒。

>ATM通常通过公共交换电话线或租用线路上的某些调制解调器（例如拨号或ADSL）连接到银行或银行间网络。网络接口卡（NIC）可用作VPN连接中的高速备用。

>下面的概述图解释了复合结构图，其中包含结构化分类器的内部结构元素 - 角色，部件，连接器。

>合结构图概述显示结构化分类器的内部结构元素 - 角色，部件，连接器。

![image](https://user-images.githubusercontent.com/30850497/60484988-f9854400-9ccd-11e9-8446-7e63d94fa3fd.png)

***
#### Apache Tomcat 7 Server UML Composite Structure Diagram Example
#### Apache Tomcat 7服务器 UML复合结构图示例
>这是UML 内部结构图的一个示例， 它显示了非集群Apache Tomcat 7 Server的简化复合结构。

>甲服务器元素表示一个卡塔利娜servlet容器 是Apache Tomcat 7 Web服务器。它是conf / server.xml配置文件中的单个最外层元素。Server元素可以包含可选的全局命名资源组件和一个或多个Services。

>每个Service元素都是Executors 和Connectors的组合，它们共享一个Engine组件。引擎接收并处理来自一个或多个连接器的所有请求，并将完成的响应返回到连接器，以便将其传输回Web客户端。一个或多个Host元素嵌套在Engine中。

>内部结构示例 - Apache Tomcat 7 Server

![image](https://user-images.githubusercontent.com/30850497/60485631-2e929600-9cd0-11e9-9110-66a1eaf74ca4.png)
>所述主机元素表示一个虚拟主机，这是一个网络名称的用于服务器的关联（如“ WWW 。myCompany的。的COM ”）与在其上Tomcat正在运行的特定服务器。可以将多个网络名称与同一虚拟主机关联。

>的背景信息元素表示一个Web应用程序，该特定的虚拟主机中运行。每个Web应用程序都基于Web应用程序归档（WAR）文件或包含相应解压缩内容的目录，如Servlet规范中所述。每个上下文必须具有唯一的上下文名称。

***
#### Observer Design Pattern UML Composite Structure Diagram Example
#### 观察者设计模式 UML复合结构图示例
>观察者模式是一种行为软件设计模式 ，其中主体维护一个称为观察者的订户列表，并通常通过调用其中一种方法通知它们任何状态变化。一旦收到状态改变通知，观察者可以请求该主题的当前状态。

>Observer设计模式 的协作示例 如下所示。协作的两个角色 - 提供者和观察者 - 将由Subject和Observer接口键入的分类器实例播放。可以将这些界面视为 对扮演角色的分类器的外部可观察特征的投影 。

>协作示例 - 观察者设计模式
![image](https://user-images.githubusercontent.com/30850497/60485695-63065200-9cd0-11e9-9cad-8726156c12b7.png)

>可以使用属性的替代表示法显示相同的协作。协作图标连接到每个矩形，表示作为协作属性类型的接口。每行都标有属性（角色）的名称。

>复合结构示例 - 观察者设计模式

![image](https://user-images.githubusercontent.com/30850497/60485711-75808b80-9cd0-11e9-8c61-7b3f1ccf0b09.png)
