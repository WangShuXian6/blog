#### Angular

>https://angular.cn

#### 安装 Angular CLI
```bash
sudo npm install -g @angular/cli
```
***

#### 创建工作空间和初始应用
>Angular 工作空间就是你开发应用的上下文环境。 每个工作空间包含一些供一个或多个项目使用的文件。 每个项目都是一组由应用、库或端到端（e2e）测试构成的文件

>ng new 命令来自动创建项目骨架

```bash
ng new my-app
```

>将创建下列工作空间和初始项目文件：

> - 一个新的工作空间，根目录名叫 my-app
> - 一个初始的骨架应用项目，也叫 my-app（但位于 src 子目录下）
> - 一个端到端测试项目（位于 e2e 子目录下）
> - 相关的配置文件

***
#### 启动开发服务器
```bash
cd my-app
ng serve --open
// --open（或只用 -o）选项会自动打开浏览器，并访问 http://localhost:4200/
```

***

>ng generate component message

>ng generate service message

>ng generate component content/pages/my-page

>app/content/pages/my-page

***

>Component（组件）是整个框架的核心，也是终极目标。“组件化”的意义有 2 个：一是分治，因为有了组件之后，我们可以把各种逻辑封装在组件内部，避免混在一起；二是复用，封装成组件之后不仅可以在项目内部复用，而且还可以沉淀下来跨项目复用。

>NgModule（模块）是组织业务代码的利器，按照自己的业务场景，把组件、服务、路由打包到模块里面，形成一个个的积木块，然后再用这些积木块来搭建出高楼大厦。

>Router（路由）的角色也非常重要，它有 3 个重要的作用：一是封装浏览器的 History 操作；二是负责异步模块的加载；三是管理组件的生命周期。

***

>老版本使用 AngularJS 指代，所有新版本都叫做 Angular。原因很好理解，因为老版本是用 JS 开发的，所以带一个 JS 后缀，而新版本是基于 TypeScript 开发的，带 JS 后缀不合适。

***
#### 官方版本发布计划：
>每 6 个月发布一个主版本（第一位版本号，主版本）
>每个主版本发布 1 ~ 3 个小版本（第二位版本号，Feature 版本号）
>每周发布一个补丁版本（第三位版本号，Hotfix 版本号）

***

>ng serve 是在内存里面生成项目，如果你想看到项目编译之后的产物，请运行 ng build。构建最终产品版本可以加参数，ng build --prod。

***
#### ng

>自动创建组件（ng generate component MyComponent，ng g c MyComponent），创建组件的时候也可以带路径，如 ng generate component mydir / MyComponent。

>自动创建指令：ng g d MyDirective。

>自动创建服务：ng g s MyService。

>构建项目：ng build，如果想构建最终的产品版本，可以用 ng build --prod 命令。

***

>@angular/cli 在 Windows 平台上需要依赖 Python 环境、Visual Studio 环境。

***
>VS Code 安装 Debugger for Chrome 插件

***
#### angular cli 使用 cdn 资源
##### 設定方式1
>.angular-cli.json

>Angular CLI 提供 deployUrl 的參數，該參數所設定的網址會影響 index.html 內的 main.bundle.js、vendor.bundle.js、 css 裡面的圖片等網址，這些網址會被加上 deployUrl 所設定的網址

>所以透過這個參數，就可以很簡單的將 CDN 的位置，加到現有的 script 的 src 裡。

##### 設定方式2
>建置指令掛參數

```bash
ng build --deploy-url=『CDN 網址』
```
>或是

```bash
ng build -d 『CDN 網址』
```
***
