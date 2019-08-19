#### CSS3
>[CSS3](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS3)

>CSS3是层叠样式表（Cascading Style Sheets）语言的最新版本，旨在扩展CSS2.1。

>它带来了许多期待已久的新特性， 例如圆角、阴影、gradients(渐变) 、transitions(过渡) 与 animations(动画) 。以及新的布局方式，如 multi-columns 、 flexible box 与 grid layouts。实验性特性以浏览器引擎为前缀（vendor-prefixed），应避免在生产环境中使用，或极其谨慎地使用，因为将来它们的语法和语义都有可能被更改。

>W3C 会定期的发布这些 snapshots，如 [2007][1], [2010][2], [2015][3] 或 [2017][4]

[1]:http://www.w3.org/TR/css-beijing/
[2]:http://www.w3.org/TR/css-2010/
[3]:https://www.w3.org/TR/css-2015/
[4]:https://www.w3.org/TR/css-2017/
***
#### CSS 模块状态
##### 稳定模块（Stable modules）
>有些 CSS 模块已经十分稳定，其状态为 CSSWG 规定的三个推荐品级之一：Candidate Recommendation（候选推荐）， Proposed Recommendation（建议推荐）或 Recommendation（推荐）。表明这些模块已经十分稳定，使用时也不必添加前缀， 但有些特性仍有可能在 Candidate Recommendation 阶段被放弃。

>[CSS Color Module Level 3][5] | Recommendation 自 2011 年 6 月 7 日

-- | --

>- 增加 opacity 属性，还有 hsl()， hsla()， rgba() 和 rgb() 函数来创建 <color> 值。 它还将 currentColor 关键字定义为合法的颜色值。transparent 颜色目前是真彩色 (多亏了支持 alpha 通道) 并且是 rgba(0,0,0,0.0) 的别名。它废弃了 system-color keywords(系统颜色关键字)， 它们已经不能在生产环境中使用。

[5]:https://drafts.csswg.org/css-color-3/

>[Selectors Level 3][6] | Recommendation 自 2011 年 9 月 29 日

-- | --

>增加：
>- 子串匹配的属性选择器， E[attribute^="value"]， E[attribute$="value"]， E[attribute*="value"]。

>- 新的伪类：:target， :enabled 和 :disabled， :checked， :indeterminate， :root， :nth-child 和 :nth-last-child， :nth-of-type 和 :nth-last-of-type， :last-child， :first-of-type 和 :last-of-type， :only-child 和 :only-of-type， :empty， 和 :not。

>- 伪元素使用两个冒号而不是一个来表示：:after 变为 ::after， :before 变为 ::before， :first-letter 变为 ::first-letter， 还有 :first-line 变为 ::first-line。

>- 新的 general sibling combinator(普通兄弟选择器)  ( h1~pre )。

[6]:https://drafts.csswg.org/selectors-3/

***
>在设备宽为 375px 中 1rem=100px

>100vw/375px*100

```css
html {
        font-size: 26.666667vw;
        box-sizing: border-box;
      }
```