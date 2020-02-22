>Flex项目（Flex Items）：Flex容器内的子元素

```less
.item{
    // Flex项目属性 order || flex-grow || flex-shrink || flex-basis
    // order
    // 允许Flex项目在一个Flex容器中重新排序。基本上，你可以改变Flex项目的顺序，从一个位置移动到另一个地方。
    // 这不会影响源代码。这也意味着Flex项目的位置在HTML源代码中不需要改变。order属性的默认值是0。它可以接受一个正值，也可以接受一个负值。
    // 值得注意的是，Flex项目会根据order值重新排序。从底到高。
    // 默认情况下，Flex项目2、3、4的order值为0。现在Flex项目1的order值为1
    // 如果两个以下Flex项目有相同的order值时，Flex项目重新排序是基于HTML源文件的位置进行排序
    
    // flex-grow和flex-shrink属性控制Flex项目在容器有多余的空间如何放大（扩展），在没有额外空间又如何缩小
    // 他们可能接受0或者大于0的任何正数。0 || positive number

    // flex-grow 在容器有多余的空间如何放大（扩展）
    // flex-grow属性值设置为0。表示Flex项目不会增长，填充Flex容器可用空间。取值为0就是一个开和关的按钮。表示flex-grow开关是关闭的
    // 把flex-grow的值设置为1,Flex项目扩展了，占据了Flex容器所有可用空间。也就是说开关打开了。如果你试着调整你浏览器的大小，Flex项目也会缩小，以适应新的屏幕宽度。

    // 默认情况下，flex-shrink的值是1，也就是说flex-shrink开关也是打开的
    // 在没有额外空间如何缩小

    // flex-basis属性可以指定Flex项目的初始大小。也就是flex-grow和flex-shrink属性调整它的大小以适应Flex容器之前
    // flex-basis默认的值是auto。flex-basis可以取任何用于width属性的任何值。比如 % || em || rem || px等
    // 如果flex-basis属性的值是0时，也需要使用单位。即flex-basis: 0px不能写成flex-basis:0
    // 默认情况，Flex项目的初始宽度由flex-basis的默认值决定，即：flex-basis: auto。Flex项目宽度的计算是基于内容的多少来自动计算（很明显，加上了padding值）。
    // 这意味着，如果你增加Flex项目中的内容，它可以自动调整大小
    // 将Flex项目设置一个固定的宽度 flex-basis: 150px;

    // flex是flex-grow、flex-shrink和flex-basis三个属性的速记（简写）
    flex: 0 1 auto;
    // 上面的代码相当于
    flex-grow: 0; // 如何扩展 0表示关闭扩展
    flex-shrink: 1; // 如何缩小 
    flex-basis: auto; // 初始大小

    // 只设置了flex-grow和flex-shrink值，flex-basis可能是默认值0。这就是所谓的绝对flex项目。
    // 只有当你设置了flex-basis，你会得到一个相对flex项目

    flex: 0 1 auto;
    // 这相当于写了flex默认属性值以及所有的Flex项目都是默认行为
    // flex-basis设置为auto，这意味着Flex项目的初始宽度计算是基于内容的大小。
    // flex-grow设置为0。这意味着flex-grow不会改变Flex项目的初始宽度。也就是说，flex-grow的开关是关闭的。
    // flex-grow控制Flex项目的增长，如果其值设置为0，Flex项目不会放大以适应屏幕（Flex容器大小）
    // flex-shrink的值是1。也就是说，Flex项目在必要时会缩小
    // Flex项目没有增长（宽度）。如果有必要，如果调整浏览器(调小浏览器宽度)，Flex项目会自动计算宽度

    flex: 0 0 auto;
    // 相当于 flex: none
    // 宽度是被自动计算，不过弹性项目不会伸展或者收缩（因为二者都被设置为零）。伸展和收缩开关都被关掉了
    // 两个弹性项目的。一个弹性项目会比另一个容纳更多内容。
    // 宽度是基于内容宽度而自动计算的
    // 缩放一下浏览器，你会注意到弹性项目不会收缩其宽度。它们从父元素中突出来了，要看到所有内容，必须横向滚动浏览器

    flex: 1 1 auto;
    // 与 flex: auto 项目相同
    // 自动计算初始化宽度，但是如果有必要，会伸展或者收缩以适应整个可用宽度
    // 伸展和收缩开关打开了，宽度自动被计算
    // 此时，项目会填满可用空间，在缩放浏览器时也会随之收缩
    // flex: 2 1 0 与写为 flex: 2 是一样的，2 表示任何正数。
    flex: 2 1 0; //与 flex: 2; 相同
    // 即，将弹性项目的初始宽度设置为零（嗯？没有宽度？），伸展项目以填满可用空间，并且最后只要有可能就收缩项目
    // 弹性项目没有宽度，那么宽度该如何计算呢？
    // 这个时候 flex-grow 值就起作用了，它决定弹性项目变宽的程度。由它来负责没有宽度的问题。
    // 当有多个弹性项目，并且其初始宽度 flex-basis 被设置为基于零的任何值时，比如 0px，使用这种 flex 简写更实用
    // 实际发生的是，弹性项目的宽度被根据 flex-grow 值的比例来计算。

    flex-grow : 1;
    // 会让弹性项目填满可用空间。伸展开关打开了。

    // 两个弹性项目。一个的 flex-grow 属性值是 1，另一个是 2
    // 两个项目上的伸展开关都打开了。不过，伸展度是不同的，1 和 2。
    // 二者都会填满可用空间，不过是按比例的。
    // 它是这样工作的：前一个占 1/3 的可用空间，后一个占 2/3 的可用空间
    // 根据基本的数学比例。"单项比例 / 总比例"
    // 即使两个弹性项目内容一样大（近似），它们所占空间还是不同。宽度不是基于内容的大小，而是伸展值。一个是另一个的约两倍

    // align-self : auto || flex-start || flex-end || center || baseline || stretch
    // 改变一个弹性项目沿着侧轴的位置，而不影响相邻的弹性项目
    // flex-end将目标项目（Flex项目）对齐到Cross-Axis的末端
    // center将目标项目（Flex项目）对齐到Cross-Axis的中间
    // stretch会将目标项目拉伸，以沿着Cross-Axis填满Flex容器的可用空间（Flex项目高度和Flex容器高度一样）
    // baseline将目标项目沿着基线对齐。它与flex-start的效果看起来是一样的
    // auto 是将目标Flex项目的值设置为父元素的 align-items值，或者如果该元素没有父元素的话，就设置为 stretch



}

```

```less
ul {
    display: flex;
    border: 1px solid red;
    padding: 0;
    list-style: none;
    justify-content: space-between;
    align-items: flex-start;
    /* 影响所有弹性项目 */
    min-height: 50%;
    background-color: #e8e8e9;
}

li {
    width: 100px;
    background-color: #8cacea;
    margin: 8px;
    font-size: 2rem;
}
```