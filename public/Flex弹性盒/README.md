#### Flex 弹性盒
>Flex容器（Flex Container）：父元素显式设置了display:flex

```less
.Container{
    // Flex容器属性  flex-direction || flex-wrap || flex-flow || justify-content || align-items || align-content
    // flex-direction属性控制Flex项目沿着主轴（Main Axis）的排列方向。
    // flex-direction: row || column || row-reverse || column-reverse;
    // 行（水平）、列（垂直）或者行和列的反向
    // 水平和垂直在Flex世界中不是什么方向（概念）。它们常常被称为主轴（Main-Axis）和侧轴（Cross-Axis）

    // flex-wrap: wrap || nowrap || wrap-reverse;
    // flex-wrap属性的默认值是nowrap。也就是说，Flex项目在Flex容器内不换行排列。
    // 希望Flex容器内的Flex项目达到一定数量时，能换行排列,把它（flex-wrap）的值设置为wrap就有这种可能
    // 当一行再不能包含所有列表项的默认宽度，他们就会多行排列。即使调整浏览器大小
    // wrap-reverse 让Flex项目在容器中多行排列，只是方向是反的
    // 789
    // 123456

    // flex-flow: row wrap;
    // flex-flow是flex-direction和flex-wrap两个属性的速记属性

    // justify-content: flex-start || flex-end || center || space-between || space-around;
    // justify-content属性主要定义了Flex项目在Main-Axis上的对齐方式。
    // justify-content的默认属性值是flex-start
    // flex-start让所有Flex项目靠Main-Axis开始边缘（左对齐）
    // flex-end让所有Flex项目靠Main-Axis结束边缘（右对齐）
    // center让所有Flex项目排在Main-Axis中间（居中对齐）
    // space-between让除了第一个和最一个Flex项目的两者间间距相同（两端对齐）
    // space-around让每个Flex项目具有相同的空间
    // 和space-between有点不同，space-around使第一个Flex项目和最后一个Flex项目距Main-Axis开始边缘和结束边缘的的间距是其他相邻Flex项目间距的一半。

    // align-items属性类似于justify-content属性
    // align-items: flex-start || flex-end || center || stretch || baseline;
    // 用来控制Flex项目在Cross-Axis对齐方式。这也是align-items和justify-content两个属性之间的不同之处
    // align-items的默认值是stretch。让所有的Flex项目高度和Flex容器高度一样
    // flex-start让所有Flex项目靠Cross-Axis开始边缘（顶部对齐）
    // flex-end让所有Flex项目靠Cross-Axis结束边缘（底部对齐）
    // center让Flex项目在Cross-Axis中间（居中对齐）
    // baseline 让所有Flex项目在Cross-Axis上沿着他们自己的基线对齐。看上去有点像flex-start，但略有不同

    // align-content属性用于多行的Flex容器。它也是用来控制Flex项目在Flex容器里的排列方式，排列效果和align-items值一样，但除了baseline属性值
    // 默认值是stretch
    // stretch会拉伸Flex项目，让他们沿着Cross-Axis适应Flex容器可用的空间
    // flex-start。这次是让多行Flex项目靠Cross-Axis开始边缘。沿着Cross-Axis从上到下排列。因此Flex项目在Flex容器中顶部对齐。
    // flex-end刚好和flex-start相反，让多行Flex项目靠着Cross-Axis结束位置。让Flex项目沿着Cross-Axis从下到上排列，即底部对齐。
    // center让多行Flex项目在Cross-Axis中间。在Flex容器中居中对齐。





    display:flex;
}
```