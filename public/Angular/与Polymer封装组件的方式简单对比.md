#### 与 Polymer 封装组件的方式简单对比

>一些开发者认为 Angular 的组件设计不如 Polymer 那种直接继承原生 HTMLElement 的方式优雅

>Polymer 组件的定义方式：

![polymer](https://user-images.githubusercontent.com/30850497/49345796-ddcca780-f6c4-11e8-9a2b-158bd8ed65ca.png)

>Polymer 的根类 Polymer.Element 的源代码：
![polymer polymer element](https://user-images.githubusercontent.com/30850497/49345804-f2a93b00-f6c4-11e8-87e3-1f4f11684eee.png)

>在 Polymer 中，开发者自定义标签的地位与浏览器原生标签完全是平等的，属性、事件、行为，都是平等的，Polymer 组件的渲染由浏览器内核直接完成。

>Polymer 的这种封装方式和目前市面上的大部分前端框架都不一样，Polymer 直接继承原生 HTML 元素，而其他大部分框架都只是在“包装”、“装饰”原生 HTML 元素，这是两种完全不同的设计哲学。

>https://www.polymer-project.org