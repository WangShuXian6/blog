#### ng路由

>Angular 中的 Router 模块会负责模块的加载、组件的初始化、销毁等操作，它是整个乐队的总指挥

#### 前端为什么要路由

> - 如果没有 Router，浏览器的前进后退按钮没法用。做过后台管理系统的开发者应该遇到过这种场景，整个系统只有一个 login.jsp 和 index.jsp，用户从 login.jsp 登录完成之后，跳转到 index.jsp 上面，然后浏览器地址栏里面的 URL 就一直停留在 index.jsp 上面，页面内部的所有内容全部通过 Ajax 进行刷新。这种处理方式实际上把浏览器的 URL 机制给废掉了，整个系统只有一个 URL，用户完全无法通过浏览器的前进、后退按钮进行导航。
> - 如果没有 Router，你将无法把 URL 复制并分享给你的朋友。比如，在某段子网站上看到了一个很搞笑的内容，你把 URL 复制下来分享给了你的朋友，如果这个段子网站没有做好路由机制，你的朋友将无法顺利打开这个链接。

>Router 的本质是记录当前页面的状态，它和当前页面上展示的内容一一对应。

>在 Angular 里面，Router 是一个独立的模块，定义在 @angular/router 模块里面，它有以下重要的作用：

> - Router 可以配合 NgModule 进行模块的懒加载、预加载操作；
> - Router 会管理组件的生命周期，它会负责创建、销毁组件。

***
#### 服务端的配置
>很多开发者会遇到这个问题：代码在开发状态运行得好好的，但是部署到真实的环境上之后所有路由都 404。

>这是一个非常典型的问题，你需要配置一下 Server 才能很好地支持前端路由。

>既然启用了前端路由，也就意味着浏览器地址栏里面的那些 URL 在 Server 端并没有真正的资源和它对应，直接访问过去当然 404 了。

>以 Tomcat 为例，需要在 web.xml 里面加一段配置：

```xml
<error-page>
    <error-code>404</error-code>
    <location>/</location>
</error-page>
```
>这意思就是告诉 Tomcat，对于 404 这种事你别管了，直接扔回前端去。由于 Angular 已经在浏览器里面接管了路由机制，因而接下来就由 Angular 来负责了。

>如果你正在使用其他的 Web 容器，请从这个[链接](https://github.com/angular-ui/ui-router/wiki/Frequently-Asked-Questions)里面查找对应的配置方式，在 How to: Configure your server to work with html5Mode 这个小节里面把常见的 Web 容器的配置方式都列举出来了，包括 IIS、Apache、Nginx、NodeJS、Tomcat 全部都有，过去抄过来就行。


>Angular 新版本的路由机制极其强大，除了能支持无限嵌套之外，还能支持模块懒加载、预加载、权限控制、辅助路由等高级功能

>Angular Router 模块的作者是 Victor Savkin，他的个人 [Blog 请单击这里](https://vsavkin.com/)，他专门编写了一本小薄书来完整描述 Angular 路由模块的设计思路和运行原理，这本书只有 151 页，如果有兴趣请点这里[查看](https://leanpub.com/router)。

#### 路由基本用法
![default](https://user-images.githubusercontent.com/30850497/49346970-7c143980-f6d4-11e8-958e-64528845b6d6.png)

>效果

![default](https://user-images.githubusercontent.com/30850497/49347207-415fd080-f6d7-11e8-8111-5504f394fab4.png)

>代码结构：

![2](https://user-images.githubusercontent.com/30850497/49347215-50df1980-f6d7-11e8-8240-f2019c8629ba.png)

>app.routes.ts 里面就是路由规则配置

```ts
import { RouterModule } from '@angular/router';
import {HomeComponent} from './home/home.component';
import {JokesComponent} from './jokes/jokes.component';

export const appRoutes=[
    {
        path:'',
        redirectTo:'home',
        pathMatch:'full'
    },
    {
        path:'home',
        component:HomeComponent
    },
    {
        path:'jokes',
        component:JokesComponent
    },
    {
        path:'**',//通配符匹配必须写在最后一个
        component:HomeComponent
    }
];
```

>app.module.ts 里面首先需要 import 这份路由配置文件：

```ts
import { appRoutes } from './app.routes';
```
>然后 @NgModule 里面的 imports 配置项内容如下：

```ts
import { appRoutes } from './app.routes';
```
```ts
imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes,{enableTracing:true})
]
```
>HTML 模板

![html](https://user-images.githubusercontent.com/30850497/49347258-a0bde080-f6d7-11e8-918b-a992ab630a03.png)
> - 整个导航过程是通过 RouterModule、app.routes.ts、routerLink、router-outlet 这几个东西一起配合完成的。
> - 请单击顶部导航条，观察浏览器地址栏里面 URL 的变化，这里体现的是 Router 模块最重要的作用，就是对 URL 和对应界面状态的管理。
> - 请注意路由配置文件 app.routes.ts 里面的写法，里面全部用的 component 配置项，这种方式叫“同步路由”。也就是说，@angular/cli 在编译的时候不会把组件切分到独立的 chunk（块）文件里面去，当然也不会异步加载，所有的组件都会被打包到一份 js 文件里面去

> - 通配符配置必须写在最后一项，否则会导致路由无效。

***

#### 路由与懒加载模块
![default](https://user-images.githubusercontent.com/30850497/49347280-eaa6c680-f6d7-11e8-8131-3e2d13f44b11.png)

>提升 JS 文件的加载速度、提升 JS 文件的执行效率。

>对于一些大型的后台管理系统来说，里面可能会有上千份 JS 文件，如果把所有 JS 全部都压缩到一份文件里面，那么这份文件的体积可能会超过 5M，这是不能接受的，尤其对于移动端应用。

>因此，一个很自然的想法就是：我们能不能按照业务功能把这些 JS 打包成多份 JS 文件，当用户导航到某个路径的时候，再去异步加载对应的 JS 文件。对于大型的系统来说，用户在使用的过程中不太可能会用到所有功能，所以这种方式可以非常有效地提升系统的加载和运行效率。

>我们来把上面这个简单的例子改成异步模式，把“主页”和“段子”切分成两个独立的模块，并且做成异步加载的模式。

>整体代码结构改成这样：

![default](https://user-images.githubusercontent.com/30850497/49347295-0d38df80-f6d8-11e8-9da6-d9e173e9a5a6.png)
>给 home 和 jokes 分别加了一个 module 文件和一个 routes 文件。

>home.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';
import { homeRoutes } from './home.routes';

@NgModule({
  declarations: [
    HomeComponent
  ],
  imports: [
    RouterModule.forChild(homeRoutes)
  ],
  providers: [],
  bootstrap: []
})
export class HomeModule { }
```

>home.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';

export const homeRoutes=[
    {
        path:'',
        component:HomeComponent
    }
];
```

>jokes 模块相关的代码类似


>最重要的修改在 app.routes.ts 里面，路由的配置变成了这样：

```ts
import { RouterModule } from '@angular/router';

export const appRoutes=[
    {
        path:'',
        redirectTo:'home',
        pathMatch:'full'
    },
    {
        path:'home',
        loadChildren:'./home/home.module#HomeModule'
    },
    {
        path:'jokes',
        loadChildren:'./jokes/jokes.module#JokesModule'
    },
    {
        path:'**',
        loadChildren:'./home/home.module#HomeModule'
    }
];
```

>我们把原来的 component 配置项改成了 loadChildren。

***

#### N 层嵌套路由

>修改上面的例子，给它加一个二级菜单
![default](https://user-images.githubusercontent.com/30850497/49347332-5a1cb600-f6d8-11e8-9f49-96dd4f659793.png)

>于是 home 模块的代码结构变成了这样：
![home](https://user-images.githubusercontent.com/30850497/49347338-69036880-f6d8-11e8-96ed-a8997b990eae.png)

>重点的变化在 home.routes.ts 里面：

```ts
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';
import { PictureComponent } from './picture/picture.component';
import { TextComponent } from './text/text.component';

export const homeRoutes=[
    {
        path:'',
        component:HomeComponent,
        children:[
            {
                path:'',
                redirectTo:'pictures',
                pathMatch:'full'
            },
            {
                path:'pictures',
                component:PictureComponent
            },
            {
                path:'text',
                component:TextComponent
            }
        ]
    }
];
```
>理论上，路由可以无限嵌套

***

#### 共享模块
>在页面的侧边栏上面加一个展示用户资料的 Panel（面板）
>这个展示用户资料的 Panel 在“段子”这个模块里面也要用，而且未来还可能在其他地方也要使用。
![default](https://user-images.githubusercontent.com/30850497/49347350-951ee980-f6d8-11e8-9333-a09d8ae589b3.png)

>根据 Angular 的规定：组件必须定义在某个模块里面，但是不能同时属于多个模块。

>如果你把这个 UserInfo 面板定义在 home.module 里面，jokes.module 就不能使用了，反之亦然。

>把 UserInfo 定义在根模块 app.module 里面
>确实可以这样做。但是这样会造成一个问题：如果系统的功能不断增多，你总不能把所有共用的组件都放到 app.module 里面吧？如果真的这样搞，app.module 最终打包出来会变得非常胖。

>更优雅的做法是切分一个“共享模块”出来，就像这样：
![default](https://user-images.githubusercontent.com/30850497/49347374-c5ff1e80-f6d8-11e8-80ca-6b58c3510c64.png)
>对于所有想使用 UserInfo 的模块来说，只要 import 这个 SharedModule 就可以了

***

#### 处理路由事件

>Angular 的路由上面暴露了 7 个事件：NavigationStart 、RoutesRecognized、RouteConfigLoadStart 、RouteConfigLoadEnd、NavigationEnd、NavigationCancel、NavigationError

>https://angular.io/guide/router#router-events

>我们可以监听这些事件，来实现一些自己的业务逻辑，示例如下：

```ts
import { Component, OnInit } from '@angular/core';
import { Router,NavigationStart } from '@angular/router';

@Component({
  selector: 'home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})
export class HomeComponent implements OnInit {

  constructor(private router: Router) {

  }

  ngOnInit() {
    this.router.events.subscribe((event) => {
      console.log(event);
      //可以用instanceof来判断事件的类型，然后去做你想要做的事情
      console.log(event instanceof NavigationStart);
    });
  }
}
```
>https://angular.io/guide/router#router-events

***

#### 如何传递和获取路由参数
>Angular 的 Router 可以传递两种类型的参数：简单类型的参数、“矩阵式”参数。

>routerLink 的写法

```html
<ul class="nav navbar-nav">
    <li routerLinkActive="active" class="dropdown">
        <a [routerLink]="['home','1']">主页</a>
    </li>
    <li routerLinkActive="active" class="dropdown">
        <a [routerLink]="['jokes',{id:111,name:'damo'}]">段子</a>
    </li>
</ul>
```

>在 HomeComponent 里面，我们是这样来获取简单参数的：

```ts
constructor(
    public router:Router,
    public activeRoute: ActivatedRoute) { 

}

ngOnInit() {
    this.activeRoute.params.subscribe(
        (params)=>{console.log(params)}
    );
}
```

>在 JokesComponent 里面，我们是这样来接受“矩阵式”参数的：

```ts
constructor(
    public router: Router,
    public activeRoute: ActivatedRoute) {

}

ngOnInit() {
    this.activeRoute.params.subscribe(
        (params) => { console.log(params) }
    );
}
```

>矩阵式”传参 [routerLink]="['jokes',{id:111,name:'damo'}]" 对应的 URL 是这样一种形态：
```
http://localhost:4200/jokes;id=111;name=damo
```
>是合法的。它不是 W3C 的规范，但是互联网之父 Tim Berners-Lee 在 1996 年的文档里面有详细的解释，主流浏览器都是支持的

>这种方式的好处是，我们可以传递大块的参数，因为第二个参数可以是一个 JSON 格式的对象。

***

#### 用代码触发路由导航
>除了通过 <a routerLink="home"> 主页 </a> 这种方式进行导航之外，还可以通过代码的方式来手动进行导航：

```ts
this.router.navigate(["/jokes"],{ queryParams: { page: 1,name:222 } });
```

>接受参数的方式如下：

```ts
this.activeRoute.queryParams.subscribe(
    (queryParam) => { console.log(queryParam) }
);
```

***

#### basic
>/src/app/app.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { JokesComponent } from './jokes/jokes.component';

export const appRoutes = [
    {
        path: '',
        redirectTo: 'home',
        pathMatch: 'full'
    },
    {
        path: 'home',
        component: HomeComponent
    },
    {
        path: 'jokes',
        component: JokesComponent
    },
    {
        path: '**',//通配符匹配必须写在最后一个
        component: HomeComponent
    }
];

```
>/src/app/app.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { JokesComponent } from './jokes/jokes.component';

import { appRoutes } from './app.routes';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    JokesComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes,{enableTracing:true})
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
>/src/app/app.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
}

```
>/src/app/app.component.html

```html
<!-- 顶部导航 -->
<div class="navbar navbar-fixed-top main-nav" role="navigation">
  <div class="container">
      <div class="navbar-header">
          <button #button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-responsive-collapse">
              <span class="sr-only">Toggle Navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
          </button>
          <a routerLink="home" class="navbar-brand navbar-brand-my">
              NiceFish
          </a>
      </div>
      <div class="collapse navbar-collapse navbar-responsive-collapse" aria-expanded="false">
          <ul class="nav navbar-nav">
              <li routerLinkActive="active" class="dropdown">
                  <a routerLink="home">主页</a>
              </li>
              <li routerLinkActive="active" class="dropdown">
                <a routerLink="jokes">段子</a>
              </li>
          </ul>
      </div>
  </div>
</div>
<!-- 主体内容区域 -->
<div class="container main-container">
  <router-outlet></router-outlet>
</div>
<!-- 底部区域 -->
<div class="footer bs-footer">
  <div class="container">
      <div class="row">
          <p>
              Powered by <a href="http://git.oschina.net/mumu-osc/NiceFish" target="_blank">NiceFish</a>
          </p>
      </div>
  </div>
</div>

```

>/src/app/home/home.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})
export class HomeComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/src/app/home/home.component.html

```html
<h3>
  这里是首页。
</h3>
<div class="list-group">
  <a href="#" class="list-group-item active">
    Cras justo odio
  </a>
  <a href="#" class="list-group-item">Dapibus ac facilisis in</a>
  <a href="#" class="list-group-item">Morbi leo risus</a>
  <a href="#" class="list-group-item">Porta ac consectetur ac</a>
  <a href="#" class="list-group-item">Vestibulum at eros</a>
</div>
```

>/src/app/jokes/jokes.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'jokes',
  templateUrl: './jokes.component.html',
  styleUrls: ['./jokes.component.scss']
})
export class JokesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/src/app/jokes/jokes.component.html

```html
<h3>
  这里是段子。
</h3>

```

***

#### async-module
![async-module](https://user-images.githubusercontent.com/30850497/49349196-79b9db80-f6e4-11e8-97ab-619d02a02b46.jpg)

>/src/app/app.routes.ts

```ts
import { RouterModule } from '@angular/router';

export const appRoutes=[
    {
		path:'',
		redirectTo:'home',
		pathMatch:'full'
	},
    {
        path:'home',
        loadChildren:'./home/home.module#HomeModule'
    },
    {
        path:'jokes',
        loadChildren:'./jokes/jokes.module#JokesModule'
    },
    {
		path:'**',
		loadChildren:'./home/home.module#HomeModule'
	}
];

```
>/src/app/app.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';
import { appRoutes } from './app.routes';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes)
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
>/src/app/app.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
}

```
>/src/app/app.component.html

```html
<!-- 顶部导航 -->
<div class="navbar navbar-fixed-top main-nav" role="navigation">
  <div class="container">
      <div class="navbar-header">
          <button #button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-responsive-collapse">
              <span class="sr-only">Toggle Navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
          </button>
          <a routerLink="home" class="navbar-brand navbar-brand-my">
              NiceFish
          </a>
      </div>
      <div class="collapse navbar-collapse navbar-responsive-collapse" aria-expanded="false">
          <ul class="nav navbar-nav">
              <li routerLinkActive="active" class="dropdown">
                  <a routerLink="home">主页</a>
              </li>
              <li routerLinkActive="active" class="dropdown">
                <a routerLink="jokes">段子</a>
              </li>
          </ul>
      </div>
  </div>
</div>
<!-- 主体内容区域 -->
<div class="container main-container">
  <router-outlet></router-outlet>
</div>
<!-- 底部区域 -->
<div class="footer bs-footer">
  <div class="container">
      <div class="row">
          <p>
              Powered by <a href="http://git.oschina.net/mumu-osc/NiceFish" target="_blank">NiceFish</a>
          </p>
      </div>
  </div>
</div>

```


>/src/app/jokes/jokes.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { JokesComponent } from './jokes.component';

export const jokesRoutes=[
    {
		path:'',
        component:JokesComponent
	}
];
```
>/src/app/jokes/jokes.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { JokesComponent } from './jokes.component';
import { jokesRoutes } from './jokes.routes';

@NgModule({
  declarations: [
    JokesComponent
  ],
  imports: [
    RouterModule.forChild(jokesRoutes)
  ],
  providers: [],
  bootstrap: []
})
export class JokesModule { }

```
>/src/app/jokes/jokes.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'jokes',
  templateUrl: './jokes.component.html',
  styleUrls: ['./jokes.component.scss']
})
export class JokesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/src/app/jokes/jokes.component.html

```html
<p>
  这里是段子！
</p>

```


>/src/app/home/home.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';

export const homeRoutes=[
    {
		path:'',
        component:HomeComponent
	}
];

```
>/src/app/home/home.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';
import { homeRoutes } from './home.routes';

@NgModule({
  declarations: [
    HomeComponent
  ],
  imports: [
    RouterModule.forChild(homeRoutes)
  ],
  providers: [],
  bootstrap: []
})
export class HomeModule { }

```
>/src/app/home/home.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})
export class HomeComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/src/app/home/home.component.html

```html

<p>
  默认跳转到首页！
</p>

```

***

#### nested-router
![nested-router](https://user-images.githubusercontent.com/30850497/49349422-7ecb5a80-f6e5-11e8-9c90-aea447f1dc95.jpg)

>/src/app/home/home.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';
import { PictureComponent } from './picture/picture.component';
import { TextComponent } from './text/text.component';

export const homeRoutes = [
    {
        path: '',
        component: HomeComponent,
        children: [
            {
                path: '',
                redirectTo: 'pictures',
                pathMatch: 'full'
            },
            {
                path: 'pictures',
                component: PictureComponent
            },
            {
                path: 'text',
                component: TextComponent
            },
            {
                path: '**',
                component: PictureComponent
            }
        ]
    }
];

```
>/src/app/home/home.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';
import { homeRoutes } from './home.routes';
import { PictureComponent } from './picture/picture.component';
import { TextComponent } from './text/text.component';

@NgModule({
  declarations: [
    HomeComponent,
    PictureComponent,
    TextComponent
  ],
  imports: [
    RouterModule.forChild(homeRoutes)
  ],
  providers: [],
  bootstrap: []
})
export class HomeModule { }

```

***

#### shared-module
![shared-module](https://user-images.githubusercontent.com/30850497/49349636-7f182580-f6e6-11e8-80d5-6ca6f65a2b52.jpg)

>/Users/wangshuxian/test/learn-router/src/app/app.routes.ts

```ts
import { RouterModule } from '@angular/router';

export const appRoutes=[
    {
		path:'',
		redirectTo:'home',
		pathMatch:'full'
	},
    {
        path:'home',
        loadChildren:'./home/home.module#HomeModule'
    },
    {
        path:'jokes',
        loadChildren:'./jokes/jokes.module#JokesModule'
    },
    {
		path:'**',
		loadChildren:'./home/home.module#HomeModule'
	}
];

```
>/Users/wangshuxian/test/learn-router/src/app/app.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';
import { appRoutes } from './app.routes';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes)
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
>/Users/wangshuxian/test/learn-router/src/app/app.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'app';
}

```
>/Users/wangshuxian/test/learn-router/src/app/app.component.html

```html
<!-- 顶部导航 -->
<div class="navbar navbar-fixed-top main-nav" role="navigation">
  <div class="container">
      <div class="navbar-header">
          <button #button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-responsive-collapse" (click)="toggle(button)">
              <span class="sr-only">Toggle Navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
          </button>
          <a routerLink="home" class="navbar-brand navbar-brand-my">
              NiceFish
          </a>
      </div>
      <div class="collapse navbar-collapse navbar-responsive-collapse" aria-expanded="false">
          <ul class="nav navbar-nav">
              <li routerLinkActive="active" class="dropdown">
                  <a routerLink="home">主页</a>
              </li>
              <li routerLinkActive="active" class="dropdown">
                <a routerLink="jokes">段子</a>
              </li>
          </ul>
      </div>
  </div>
</div>
<!-- 主体内容区域 -->
<div class="container main-container">
  <router-outlet></router-outlet>
</div>
<!-- 底部区域 -->
<div class="footer bs-footer">
  <div class="container">
      <div class="row">
          <p>
              Powered by <a href="http://git.oschina.net/mumu-osc/NiceFish" target="_blank">NiceFish</a>
          </p>
      </div>
  </div>
</div>

```


>/Users/wangshuxian/test/learn-router/src/app/shared/shared.module.ts

```ts
import { NgModule } from '@angular/core';
import { UserInfoComponent } from '../user-info/user-info.component';
import { OrderInfoComponent } from '../order-info/order-info.component';

@NgModule({
  declarations: [
    UserInfoComponent,
    OrderInfoComponent
  ],
  imports: [],
  exports:[
    UserInfoComponent,
    OrderInfoComponent
  ],
  providers: [],
  bootstrap: []
})
export class SharedModule { }


```


>/Users/wangshuxian/test/learn-router/src/app/user-info/user-info.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'user-info',
  templateUrl: './user-info.component.html',
  styleUrls: ['./user-info.component.scss']
})
export class UserInfoComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/Users/wangshuxian/test/learn-router/src/app/user-info/user-info.component.html

```html
<div class="panel panel-primary">
  <div class="panel-heading">用户资料</div>
  <div class="panel-body">
    这是用来展示用户资料的组件
  </div>
</div>
```


>/Users/wangshuxian/test/learn-router/src/app/order-info/order-info.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'order-info',
  templateUrl: './order-info.component.html',
  styleUrls: ['./order-info.component.scss']
})
export class OrderInfoComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/Users/wangshuxian/test/learn-router/src/app/order-info/order-info.component.html

```html
<div class="panel panel-primary">
  <div class="panel-heading">订单数据</div>
  <div class="panel-body">
    这里用来展示订单数据！
  </div>
</div>
```


>/Users/wangshuxian/test/learn-router/src/app/jokes/jokes.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { JokesComponent } from './jokes.component';

export const jokesRoutes=[
    {
		path:'',
        component:JokesComponent
	}
];
```
>/Users/wangshuxian/test/learn-router/src/app/jokes/jokes.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { SharedModule } from '../shared/shared.module';
import { JokesComponent } from './jokes.component';
import { jokesRoutes } from './jokes.routes';

@NgModule({
  declarations: [
    JokesComponent
  ],
  imports: [
    SharedModule,
    RouterModule.forChild(jokesRoutes)
  ],
  providers: [],
  bootstrap: []
})
export class JokesModule { }

```
>/Users/wangshuxian/test/learn-router/src/app/jokes/jokes.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'jokes',
  templateUrl: './jokes.component.html',
  styleUrls: ['./jokes.component.scss']
})
export class JokesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/Users/wangshuxian/test/learn-router/src/app/jokes/jokes.component.html

```html
<div class="row">
  <div class="col-xs-3">
    <user-info></user-info>
  </div>
  <div class="col-xs-9">
    <h3>
      这里是段子！
    </h3>
  </div>
</div>
<!-- <order-info></order-info> -->
```


>/Users/wangshuxian/test/learn-router/src/app/home/home.routes.ts

```ts
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';

export const homeRoutes=[
    {
		path:'',
        component:HomeComponent
	}
];

```
>/Users/wangshuxian/test/learn-router/src/app/home/home.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { SharedModule } from '../shared/shared.module';
import { HomeComponent } from './home.component';
import { homeRoutes } from './home.routes';

@NgModule({
  declarations: [
    HomeComponent    
  ],
  imports: [
    SharedModule,
    RouterModule.forChild(homeRoutes)
  ],
  providers: [],
  bootstrap: []
})
export class HomeModule { }

```
>/Users/wangshuxian/test/learn-router/src/app/home/home.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})
export class HomeComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```
>/Users/wangshuxian/test/learn-router/src/app/home/home.component.html

```html
<div class="row">
  <div class="col-xs-3">
    <user-info></user-info>
  </div>
  <div class="col-xs-9">
    <h3>
      默认跳转到首页！
    </h3>
  </div>
</div>
<!-- <order-info></order-info> -->
```


***

#### router-events

>

***

#### router-params

![router-params](https://user-images.githubusercontent.com/30850497/49350440-eedbdf80-f6e9-11e8-8bbb-f1025fcf69d0.jpg)

>/Users/wangshuxian/test/learn-router/src/app/app.routes.ts

```ts
import { RouterModule } from '@angular/router';
import {HomeComponent} from './home/home.component';
import {JokesComponent} from './jokes/jokes.component';

export const appRoutes=[
    {
		path:'',
		redirectTo:'home/1',
		pathMatch:'full'
	},
    {
        path:'home/:page',
        component:HomeComponent
    },
    {
        path:'jokes',
        component:JokesComponent
    },
    {
		path:'**',
		component:HomeComponent
	}
];

```
>/Users/wangshuxian/test/learn-router/src/app/app.module.ts

```ts
import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';
import { appRoutes } from './app.routes';
import { HomeComponent } from './home/home.component';
import { JokesComponent } from './jokes/jokes.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    JokesComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes)
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
>/Users/wangshuxian/test/learn-router/src/app/app.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {

}

```
>/Users/wangshuxian/test/learn-router/src/app/app.component.html

```html
<!-- 顶部导航 -->
<div class="navbar navbar-fixed-top main-nav" role="navigation">
    <div class="container">
        <div class="navbar-header">
            <button #button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-responsive-collapse">
              <span class="sr-only">Toggle Navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
          </button>
            <a [routerLink]="['home','1']" class="navbar-brand navbar-brand-my">
              NiceFish
          </a>
        </div>
        <div class="collapse navbar-collapse navbar-responsive-collapse" aria-expanded="false">
            <ul class="nav navbar-nav">
                <li routerLinkActive="active" class="dropdown">
                    <a [routerLink]="['home','1']">主页</a>
                </li>
                <li routerLinkActive="active" class="dropdown">
                    <a [routerLink]="['jokes',{id:111,name:'damo'}]">段子</a>
                </li>
            </ul>
        </div>
    </div>
</div>
<!-- 主体内容区域 -->
<div class="container main-container">
    <router-outlet></router-outlet>
</div>
<!-- 底部区域 -->
<div class="footer bs-footer">
    <div class="container">
        <div class="row">
            <p>
                Powered by <a href="http://git.oschina.net/mumu-osc/NiceFish" target="_blank">NiceFish</a>
            </p>
        </div>
    </div>
</div>
```


>/Users/wangshuxian/test/learn-router/src/app/jokes/jokes.component.ts

```ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Router, Params } from '@angular/router';

@Component({
  selector: 'jokes',
  templateUrl: './jokes.component.html',
  styleUrls: ['./jokes.component.scss']
})
export class JokesComponent implements OnInit {

  constructor(
    public router: Router,
    public activeRoute: ActivatedRoute
  ) { }

  ngOnInit() {
    this.activeRoute.queryParams.subscribe(
      (queryParam) => { console.log(queryParam) }
    );
    this.activeRoute.params.subscribe(
      (params) => { console.log(params) }
    );
  }
}

```
>/Users/wangshuxian/test/learn-router/src/app/jokes/jokes.component.html

```ts
<h3>
  这里是段子！
</h3>

```


>/Users/wangshuxian/test/learn-router/src/app/home/home.component.ts

```ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Router, Params } from '@angular/router';

@Component({
  selector: 'home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})
export class HomeComponent implements OnInit {

  constructor(
    public router: Router,
    public activeRoute: ActivatedRoute) {

  }

  ngOnInit() {
    this.activeRoute.params.subscribe(
      (params) => { console.log(params) }
    );
  }

  public manualNav(): void {
    // this.router.navigateByUrl("/jokes");
    //navigate方法不支持矩阵式参数
    this.router.navigate(["/jokes"], { queryParams: { page: 1, name: 222 } });
  }
}

```
>/Users/wangshuxian/test/learn-router/src/app/home/home.component.html

```html
<h3>
  默认跳转到首页！
</h3>
<button class="btn btn-primary" (click)="manualNav()">手动导航</button>
```


***

#### 