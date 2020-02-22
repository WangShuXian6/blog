#### Markdown 基本语法

http://www.markdown.cn

Markdown语法主要分为如下几大部分： 标题，段落，区块引用，代码区块，强调，列表，分割线，链接，图片，反斜杠 \，符号'`'。

4.1 标题

两种形式：
1）使用=和-标记一级和二级标题。

一级标题
=========
二级标题
---------
效果：

一级标题

二级标题
2）使用#，可表示1-6级标题。

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
效果：

一级标题

二级标题

三级标题

四级标题

五级标题

六级标题
4.2 段落

段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用两个以上空格加上回车（引用中换行省略回车）。

4.3 区块引用

在段落的每行或者只在第一行使用符号>,还可使用多个嵌套引用，如：

> 区块引用
>> 嵌套引用
效果：

区块引用

嵌套引用
4.4 代码区块

代码区块的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。如
普通段落：

void main()
{
printf("Hello, Markdown.");
}

代码区块：

void main()
{
    printf("Hello, Markdown.");
}
注意:需要和普通段落之间存在空行。

4.5 强调

在强调内容两侧分别加上*或者_，如：

*斜体*，_斜体_
**粗体**，__粗体__
效果：

斜体，斜体
粗体，粗体
4.6 列表

使用·、+、或-标记无序列表，如：

-（+*） 第一项 -（+*） 第二项 - （+*）第三项
注意：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。

效果：

第一项
第二项
第三项
有序列表的标记方式是将上述的符号换成数字,并辅以.，如：

1 . 第一项
2 . 第二项
3 . 第三项
效果：

第一项
第二项
第三项
4.7 分割线

分割线最常使用就是三个或以上*，还可以使用-和_。

4.8 链接

链接可以由两种形式生成：行内式和参考式。
行内式：

[younghz的Markdown库](https:://github.com/younghz/Markdown "Markdown")。
效果：

younghz的Markdown库。
参考式：

[younghz的Markdown库1][1]
[younghz的Markdown库2][2]
[1]:https:://github.com/younghz/Markdown "Markdown"
[2]:https:://github.com/younghz/Markdown "Markdown"
效果：

younghz的Markdown库1
younghz的Markdown库2
注意：上述的[1]:https:://github.com/younghz/Markdown "Markdown"不出现在区块中。

4.9 图片

添加图片的形式和链接相似，只需在链接的基础上前方加一个！。
```javascript
![cardPoint](./cardPoint.jpg)
//  同级目录下的cardPoint.jpg
```
4.10 反斜杠\

相当于反转义作用。使符号成为普通符号。

4.11 符号'`'

起到标记作用。如：

`ctrl+a`
效果：

ctrl+a
5. 谁在用？

Markdown的使用者：

GitHub
简书
Stack Overflow
Apollo
Moodle
Reddit
等等
6. 尝试一下

Chrome下的插件诸如stackedit与markdown-here等非常方便，也不用担心平台受限。
在线的dillinger.io评价也不错
Windowns下的MarkdownPad也用过，不过免费版的体验不是很好。
Mac下的Mou是国人贡献的，口碑很好。
Linux下的ReText不错。
当然，最终境界永远都是笔下是语法，心中格式化 :)。

注意：不同的Markdown解释器或工具对相应语法（扩展语法）的解释效果不尽相同，具体可参见工具的使用说明。 虽然有人想出面搞一个所谓的标准化的Markdown，[没想到还惹怒了健在的创始人John Gruber] (http://blog.codinghorror.com/standard-markdown-is-now-common-markdown/)。

以上基本是所有traditonal markdown的语法。