### starUML 术语
>https://docs.staruml.io/

### Classes(basic)
>Class

![image](https://user-images.githubusercontent.com/30850497/60406101-d338a900-9be6-11e9-8989-fb71192e6c20.png)
>类

#### Interface 接口

![image](https://user-images.githubusercontent.com/30850497/60406110-e51a4c00-9be6-11e9-8b2a-19239bab7ee7.png)
>接口

>是描述类的部分行为的一组操作，它也是一个类提供给另一个类的一组操作。

>通常接口被描述为抽象操作，也就是只用标识（返回值、操作名称、参数表）说明它的行为，而真正实现部分放在使用该接口的对象中，也就是说，接口只负责定义操作而不负责具体的实现。

>接口只是一组操作，没有属性

>接口的表示和类图的表示类似，只是在最上面的一层类名前加描述＜＜interface＞＞，或是用一个圆圈表示

#### Association 关联

![image](https://user-images.githubusercontent.com/30850497/60406140-07ac6500-9be7-11e9-8bd5-ed87df1e73ae.png)
>关联指明一个对象与另一个对象之间的关系。在图形中，关联用一条实线表示，它可能有方向，偶尔在其上还有一个标记

>Directed Association

![image](https://user-images.githubusercontent.com/30850497/60406180-36c2d680-9be7-11e9-847f-25958f4b340e.png)
>

#### Aggregation 聚合

![image](https://user-images.githubusercontent.com/30850497/60406198-4c380080-9be7-11e9-8dac-550302cc24b6.png)
>两个类之间的简单关联表示两个同等地位的类之间的结构关系，说明这两个类是同一级别的，一个类并不比另一个类显得重要。

>在实际建模中，往往需要对整体/部分的关系进行描述，一个类描述了一个较大的事物（整体），它由较小的事物（部分）组成。

>聚合关系正是表示整体和部分关系的关联。

>聚合描述了has-a的关系，意思是整体对象拥有部分对象。

>实质上，聚合就是一种特殊的关联。

>在UML中，聚合被表示为在整体的一端用一个空心菱形修饰的简单关联。

>聚合关系用带空心菱形的实线来表示，其中头部指向整体。

>一所学校里肯定会设置多个系部

>聚合的含义完全是概念性的，空心菱形只是把整体和部分区别开，简单的聚合没有改变整体和部分之间跨越关联的导航含义，与整体和部分的生命周期也不相关

#### Composition 组合关系/组合式聚合

![image](https://user-images.githubusercontent.com/30850497/60406228-6b369280-9be7-11e9-9447-85929b36ecfe.png)
>组合关系用带实心菱形的实线来表示，其中头部指向整体

>组合关系是聚合关系中的一种特殊情况，是更强形式的聚合，又被称为强聚合。

>在组合中，成员对象的生命周期取决于聚合的生命周期，聚合不仅控制着成员对象的行为，而且控制着成员对象的创建和撤销。

>这就意味着，在组合式聚合中，一个对象在一个时间内只能是一个组合的一部分

>一个菜单只属于一个窗口

>在组合式聚合中，整体要对它的各个组成部分进行处理，也就是说，整体必须管理部分的创建和撤销。

>例如，在创建一个菜单时，必须将它附加到它所属的一个窗口中，相应地，当撤销一个窗口时，必须要依次撤销它的菜单和按钮。

#### Dependence 依赖

![image](https://user-images.githubusercontent.com/30850497/60406248-81dce980-9be7-11e9-9792-03980bf2014e.png)
>依赖是两个模型元素间的语义关系，其中一个元素（独立事务）的变化会影响另一个元素（依赖事务）的语义。

>被依赖指向依赖

#### Generalization 泛化

![image](https://user-images.githubusercontent.com/30850497/60406270-9e792180-9be7-11e9-992e-beb286370664.png)
>泛化是一种一般化到特殊化的关系，是一般事物（父类）和该事物较为特殊的种类（子类）之间的关系，子类继承父类的属性和操作，除此之外，子类还添加新的属性和操作。在图形中，泛化关系用带有空心箭头的实线表示，该实线指向父类

>Interface Realization

![image](https://user-images.githubusercontent.com/30850497/60406312-c7011b80-9be7-11e9-994b-7626b16494b1.png)
>接口实现

***
### Classes (Adavanced)
#### Signal

![image](https://user-images.githubusercontent.com/30850497/60406373-07f93000-9be8-11e9-9675-44348bec3f4d.png)
>信号

#### DataType

![image](https://user-images.githubusercontent.com/30850497/60406388-24956800-9be8-11e9-98de-2b19e27d0acc.png)
>数据类型

#### PrimitiveType

![image](https://user-images.githubusercontent.com/30850497/60406405-3e36af80-9be8-11e9-93b7-3b8b1572642d.png)
>原始类型

#### Enumeration

![image](https://user-images.githubusercontent.com/30850497/60406418-573f6080-9be8-11e9-8df8-bcef52ffdb2d.png)
>枚举

#### Frame

![image](https://user-images.githubusercontent.com/30850497/60406447-8229b480-9be8-11e9-84f2-3f19d760d1d4.png)
>

#### Association Class 关联类

![image](https://user-images.githubusercontent.com/30850497/60406465-9bcafc00-9be8-11e9-87aa-bd0aa648415b.png)
>在两个类之间的关联中，关联本身可以有特征。即关联和类一样，也可以有自己的属性和操作。此时，这个关联实际上是一个关联类

>关联类是同时具有类特征和关系特征的模型元素，可以将关联类看成是拥有类特性的关联，也可以把它看成是拥有关联特性的类。关联类的可视化表示方式与一般的类相同，但是要用一条虚线把关联类和对应的关联线连接起来。关联类也可以与其他类关联。

***

### Packages

#### Package 设计包

![image](https://user-images.githubusercontent.com/30850497/60406486-b1402600-9be8-11e9-856b-af6b302b2fc6.png)
>ＭODULE（也称为PACKAGE）

>MODULE是一个传统的、较成熟的设计元素。虽然使用模块有一些技术上的原因，但主要原因却是‚认知超载‛ [6]。MODULE为人们提供了两种观察模型的方式，一是可以在MODULE中查看细节，而不会被整个模型淹没，二是观察MODULE之间的关系，而不考虑其内部细节。
>MODULE之间应该是低耦合的，而在MODULE的内部则是高内聚的。

>MODULE并不仅仅是代码的划分，而且也是概念的划分。一个人一次考虑的事情是有限的（因此才要低耦合）

#### Model 模型

![image](https://user-images.githubusercontent.com/30850497/60406501-bac98e00-9be8-11e9-8fd2-925040e744a6.png)


#### Subsystem 子系统/设计子系统
![image](https://user-images.githubusercontent.com/30850497/60406507-c5842300-9be8-11e9-947c-83a5a461699d.png)


#### Containment
![image](https://user-images.githubusercontent.com/30850497/60406517-d2087b80-9be8-11e9-91d5-7ae329f6e40a.png)


#### Dependency 依赖
![image](https://user-images.githubusercontent.com/30850497/60406531-de8cd400-9be8-11e9-943d-6604dc803f77.png)

***

### Composite Structure

#### Collaboration 合作/协作
![image](https://user-images.githubusercontent.com/30850497/60406558-ff552980-9be8-11e9-9706-fd99772ce850.png)

#### Port 端口
![image](https://user-images.githubusercontent.com/30850497/60406562-07ad6480-9be9-11e9-8c16-1018420f545e.png)
>端口将组合结构与外部环境隔离，实现了双向的封装，既涵盖了该组合结构所提供的行为（Provided Interface），同时也指出了该组合结构所需要的服务（Required Interface）；如协议（Protocol），基于UML中的协作（Collaboration）的概念，展示那些可复用的交互序列，其实质目的是描述那些可以在不同上下文环境中复用的协作模式。协议中所反映的任务由具体的端口承担。

#### Part 部件
![image](https://user-images.githubusercontent.com/30850497/60406566-10059f80-9be9-11e9-964d-04bf403cc07e.png)

#### Connector 连接器
![image](https://user-images.githubusercontent.com/30850497/60406571-1b58cb00-9be9-11e9-8830-e4b48fde19c9.png)

#### Collaboration Use
![image](https://user-images.githubusercontent.com/30850497/60406585-2875ba00-9be9-11e9-9bd0-d9d9fbb7b107.png)

#### Role binding 角色绑定
![image](https://user-images.githubusercontent.com/30850497/60406592-3297b880-9be9-11e9-880b-dbb644221dd3.png)

#### Realization 实现
![image](https://user-images.githubusercontent.com/30850497/60406594-3cb9b700-9be9-11e9-8033-05b1a1671972.png)
>实现是类之间的语义关系，其中的一个类指定了由另一个类必须执行的约定。

>在两种地方会遇到实现关系，一种是在接口和实现它们的类或构件之间，另一种是在用例和实现它们的协作之间。

>在图形中，实现关系用一条带有空心箭头的虚线表示，它是泛化和依赖关系两种图形的结合

***

### Instances 实例

#### Object 对象
![image](https://user-images.githubusercontent.com/30850497/60406607-565afe80-9be9-11e9-9741-c9a55eae084b.png)
>对象是面向对象的基本构造单元，它是系统中用来描述客观事物的一个实体。

>一个对象由一组属性和对属性执行操作的一组方法组成。

>在面向对象的系统中，对象是一个封装数据属性和操作行为的实体。数据描述了对象的状态，操作指的是操作私有数据并改变对象的状态。当其他对象向本对象发出消息，本对象响应时，其操作才得以实现，在对象内部的操作通常叫做方法。

#### Artifact instance 工件实例/产物实例

![image](https://user-images.githubusercontent.com/30850497/60406617-68d53800-9be9-11e9-9726-36fddc7b3c6a.png)

#### Component Instance 组件实例

![image](https://user-images.githubusercontent.com/30850497/60406632-768abd80-9be9-11e9-9c52-58b306008e37.png)

#### Node Instance 节点实例
![image](https://user-images.githubusercontent.com/30850497/60406637-80142580-9be9-11e9-8a24-37d9ac2001d0.png)


#### Link 链接
![image](https://user-images.githubusercontent.com/30850497/60406643-8904f700-9be9-11e9-83a9-d25b74a480ec.png)
>链接（Link）用线条来表示，链接表示两个对象共享一个消息，位于对象之间或参与者与对象之间。

>链接表示两个或多个对象间的独立连接，是关联的实例。在协作图中，关联角色是与具体语境有关的暂时的类元之间的关系，关系角色的实例也是链。链表示为一个或多个相连的线或弧。

#### Directed Link
![image](https://user-images.githubusercontent.com/30850497/60406660-9a4e0380-9be9-11e9-8065-4ed64e96dc60.png)

***

### Annotations

#### Text
![image](https://user-images.githubusercontent.com/30850497/60406700-d5503700-9be9-11e9-9f62-364f56060956.png)

#### Note
![image](https://user-images.githubusercontent.com/30850497/60406710-df723580-9be9-11e9-960f-73923859e465.png)

#### Note Link
![image](https://user-images.githubusercontent.com/30850497/60406718-edc05180-9be9-11e9-8a02-af6edfa2b28f.png)

#### Hyperlink
![image](https://user-images.githubusercontent.com/30850497/60406756-1cd6c300-9bea-11e9-8380-efaa62e9e133.png)

#### Rectangle
![image](https://user-images.githubusercontent.com/30850497/60406799-4c85cb00-9bea-11e9-9a40-5bb99a34f5fb.png)

#### Rounded Rectangle
![image](https://user-images.githubusercontent.com/30850497/60406809-5b6c7d80-9bea-11e9-8135-f864b4db9ff8.png)

#### Ellipse
![image](https://user-images.githubusercontent.com/30850497/60406824-6c1cf380-9bea-11e9-8ef4-50822a633cbd.png)

***
### Robustness

#### Boundary 边界
![image](https://user-images.githubusercontent.com/30850497/60406837-8787fe80-9bea-11e9-8ff6-3fc1f713f403.png)
>我们需要用一个抽象来封装模型中的引用。AGGREGATE就是一组相关对象的集合，我们把它作为数据修改的单元。每个AGGREGATE都有一个根（root）和一个边界（boundary）。边界定义了AGGREGATE的内部都有什么。根则是AGGREGATE所包含的一个特定ENTITY。对AGGREGATE而言，外部对象只可以引用根，而边界内部的对象之间则可以互相引用。除根以外的其他ENTITY都有本地标识，但这些标识只在AGGREGATE内部才需要加以区别，因为外部对象除了根ENTITY之外看不到其他对象。

#### Entity 实体
![image](https://user-images.githubusercontent.com/30850497/60406842-91116680-9bea-11e9-836f-86e9585c4e37.png)
>由标识定义的对象被称作ENTITY

>ENTITY（实体）有特殊的建模和设计思路。它们具有生命周期，这期间它们的形式和内容可能发生根本改变，但必须保持一种内在的连续性。为了有效地跟踪这些对象，必须定义它们的标识。它们的类定义、职责、属性和关联必须由其标识来决定，而不依赖于其所具有的属性。即使对于那些不发生根本变化或者生命周期不太复杂的ENTITY，也应该在语义上把它们作为ENTITY来对待，这样可以得到更清晰的模型和更健壮的实现。

>ENTITY可以是任何事物，只要满足两个条件即可，

>一是它在整个生命周期中具有连续性，

>二是它的区别并不是由那些对用户非常重要的属性决定的。

>ENTITY可以是一个人、一座城市、一辆汽车、一张彩票或一次银行交易。

#### Control 控制权
![image](https://user-images.githubusercontent.com/30850497/60406850-98d10b00-9bea-11e9-8ab6-04e8f852f34c.png)
