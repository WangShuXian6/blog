#### 模块 @NgModule

![default](https://user-images.githubusercontent.com/30850497/49346638-55ec9a80-f6d0-11e8-8ea5-6c0e48813a6a.png)

>https://angular.cn/guide/ngmodules


#### @NgModule 的定义方式

>一个最简单的 @NgModule 的定义：

```ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { TestViewChildComponent } from './test-view-child/test-view-child.component';
import { ChildOneComponent } from './test-view-child/child-one/child-one.component';

@NgModule({
  declarations: [
    AppComponent,
    TestViewChildComponent,
    ChildOneComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

>其中：

> - declarations，用来放组件、指令、管道的声明；
> - imports，用来导入外部模块；
> - providers，需要使用的 Service 都放在这里；
> - bootstrap，定义启动组件，你可能注意到了这个配置项是一个数组，也就是说可以指定做个组件作为启动点，但是这种用法是很罕见的。

***

#### @NgModule 的重要作用

> - NgModule 最根本的意义是帮助开发者组织业务代码，开发者可以利用 NgModule 把关系比较紧密的组件组织到一起，这是首要的。
> - NgModule 用来控制组件、指令、管道等的可见性，处于同一个 NgModule 里面的组件默认互相可见，而对于外部的组件来说，只能看到 NgModule 导出（exports）的内容，这一特性非常类似 Java 里面 package 的概念。也就是说，如果你定义的 NgModule 不 exports 任何内容，那么外部使用者即使 import 了你这个模块，也没法使用里面定义的任何内容。
> - NgModule 是 @angular/cli 打包的最小单位。打包的时候，@angular/cli 会检查所有 @NgModule 和路由配置，如果你配置了异步模块，cli 会自动把模块切分成独立的 chunk（块）。这一点是和其他框架不同的，其他框架基本上都需要你自己去配置 Webpack，自己定义切分 chunck 的规则；而在 Angular 里面，打包和切分的动作是 @angular/cli 自动处理的，不需要你干预。当然，如果你感到不爽，也可以自己从头用 Webpack 配一个环境出来，因为 @angular/cli 底层也是用的 webpack。
> - NgModule 是 Router 进行异步加载的最小单位，Router 能加载的最小单位是模块，而不是组件。当然，模块里面只放一个组件是允许的，很多组件库都是这样做的。

***

#### @NgModule 的注意点

> - 每个应用至少有一个根模块，按照惯例，根模块的名字一般都叫 AppModule，如果没有非常特别的理由，就不要随意改这个名字了，这相当于一个国际惯例。
> - 组件、指令、管道都必须属于一个模块，而且只能属于一个模块。
> - NgModule 和 ES 6 里面的 Module 是完全不同的两个概念。ES 6 里面的模块是通过 export 和 import 来进行声明的，它们是语法层面的内容；而 NgModule 完全不是这个概念，从上面的作用列表也能看出来。最重要的一点，目前，ES 6 里面的 import 只能静态引入模块，并不能异步动态加载模块，而 NgModule 可以配合 Router 来进行异步模块加载，在后面的 Router 这一课里面会有实例代码。
> - 模块的定义方式会影响依赖注入机制：对于直接 import 的同步模块，无论把 @Injectable 类型的组件定义在哪个模块里面，它都是全局可见的。比如，在子模块 post.module.ts 的 providers 数组里面定义了一个 PostListService，可能会觉得这个 PostListService 只有在 post.module.ts 里面可见。而事实并非如此，PostListService 是全局可见的，就相当于一个全局单例。与此对应，如果把 PostListService 定义到一个异步加载的模块里面，它就不是全局可见的了，因为对于异步加载进来的模块，Angular 会为它创建独立的 DI（依赖注入）上下文。所以，如果你想让 PostListService 全局可见，应该把它定义在根模块 app.module 里面。同时要特别注意，如果希望 PostListService 是全局单例的，只能在 app.module 里面的 providers 数组里面定义一次，而不能在其他模块里面再次定义，否则就会出现多个不同的实例。
>关于 DI 机制更详细的描述请参见这里。https://angular.cn/guide/dependency-injection

***

#### Angular 内核中的目录结构

>Angular 本身也是用模块化的方式开发的，当你用 cnpm install 装好了开发环境之后，可以打开 node_modules 目录查看整体结构。左侧第一列 6 个目录比较常用，这个顺序是按照我自己的理解排列的，按照常用程度从上到下。

![angular](https://user-images.githubusercontent.com/30850497/49346848-1d9a8b80-f6d3-11e8-98c8-878c5aab76b0.png)
>注意，一个目录里面可能会放多个模块，比如 forms 目录里面就有 FormsModule 和 ReactiveFormsModule 两个模块。

>Form 模块的关键类结构图如下：

![form](https://user-images.githubusercontent.com/30850497/49346853-36a33c80-f6d3-11e8-809a-89ad8c14a415.png)
