#### JavaScript 的并发模型基于"事件循环"

> https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop#事件循环

![default](https://user-images.githubusercontent.com/30850497/40952730-0ca1404c-68af-11e8-9b65-93f4f993debc.jpeg)

<hr>

#### 只包含同步函数的事件循环流程
>浏览器遇到script标签，开始解析，进行第一轮事件循环。


>每遇到一个同步函数，创建包含该函数的参数和局部变量的栈帧，压入主线程。

>运行该函数，如果函数内包含另一个函数，创建包含该函数的参数和局部变量的栈帧。压倒之前帧之上。

>最上一个栈帧开始执行，该帧返回时，即弹出栈，离开主线程。

>继续执行内层的帧，知道最内层的帧返回，栈为空。

<hr>