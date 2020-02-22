#### 绝对和相对Flex项目
>// 绝对Flex项目的宽度只基于 flex 属性，而相对Flex项目的宽度基于内容大小。

>只设置了flex-grow和flex-shrink值，flex-basis可能是默认值0。这就是所谓的绝对flex项目。

>只有当你设置了flex-basis，你会得到一个相对flex项目

#### 绝对的Flex项目

```less
/* 这是一个绝对的Flex项目 */

li {
    flex: 1 1;
    /*flex-basis 默认值为 0*/
}
```
<hr>

#### 相对的Flex项目

```less
/* 这是一个相对的Flex项目 */

li {
    flex-basis: 200px;
    /* 只设置了flex-basis的值 */
}
```
<hr>

#### 绝对的Flex项目

>把Flex项目变成绝对的, 就是说这次它们的宽度是基于 flex 属性，而不是内容的大小

```less
// 把Flex项目变成绝对的, 就是说这次它们的宽度是基于 flex 属性，而不是内容的大小
li {
    flex: 1;
    /*与 flex: 1 1 0 相同*/
}
// 两个Flex项目的宽度相同
// Flex项目的初始宽度是零（flex-basis: 0），并且它们会伸展以适应可用空间。
// 当有两到多个Flex项目的 flex-basis 取值为0时，它们会基于 flex-grow值共享可用空间。
// 现在宽度不会基于内容大小而计算，而是基于指定的 flex 属性值来计算
```
<hr>

####

>当在Flex项目上使用 margin: auto 时，值为 auto 的方向（左、右或者二者都是）会占据所有剩余空间。

>flex-grow 值为设置为0。这就解释了为什么列表项不会伸展。

>Flex项目向Main-Axis的开头对齐（这是默认行为）。

>由于项目被对齐到Main-Axis开头，右边就有一些多余的空间。看到了吧？

```less
ul {
    display: flex;
}

li {
    flex: 0 0 auto;
}
```
>在第一个列表项（branding）上使用 margin: auto

>之前的剩余空间现在已经被分配到第一个Flex项目的右边了

>当在Flex项目上使用 margin: auto 时，值为 auto 的方向（左、右或者二者都是）会占据所有剩余空间。

```less
li:nth-child(1) {
    margin-right: auto;
    /*只应用到右外边距*/
}
```

>让一个Flex项目的两边都用自动外边距对齐

>现在空白被分配到Flex项目的两边了

>注意：当在一个Flex项目上使用自动外边距（margin: auto）时，justify-content 属性就不起作用了。

```less
li:nth-child(1) {
    margin-left: auto;
    margin-right: auto
}
```