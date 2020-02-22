#### @ContentChild & @ContentChildren

>contentchild

#### @ContentChild
>我们可以利用 @ContentChild 这个装饰器来操控被投影进来的组件。

```html
<child-one>
    <child-two></child-two>
</child-one>
```
```ts
import { Component, ContentChild, ContentChildren, ElementRef, OnInit, QueryList } from '@angular/core';

//注解的写法
@ContentChild(ChildTwoComponent)
childTwo:ChildTwoComponent;

//在 ngAfterContentInit 钩子里面访问被投影进来的组件
ngAfterContentInit():void{
    console.log(this.childTwo);
    //这里还可以访问 this.childTwo 的 public 型方法，监听 this.childTwo 所派发出来的事件
}
```

***

#### @ContentChildren
>从名字可以看出来，@ContentChildren 是一个复数形式。当被投影进来的是一个组件列表的时候，我们可以用 @ContentChildren 来进行操控。

```html
<child-one>
    <child-two></child-two>
    <child-two></child-two>
    <child-two></child-two>
    <child-two></child-two>
    <child-two></child-two>
    <child-two></child-two>
    <child-two></child-two>
    <child-two></child-two>
</child-one>
```

```ts
import { Component, ContentChild, ContentChildren, ElementRef, OnInit, QueryList } from '@angular/core';

//这时候不是单个组件，是一个列表 QueryList
@ContentChildren(ChildTwoComponent) 
childrenTwo:QueryList<ChildTwoComponent>;

//遍历列表
ngAfterContentInit():void{
    this.childrenTwo.forEach((item)=>{
        console.log(item);
    });
}
```

***

![contentchildren](https://user-images.githubusercontent.com/30850497/49345628-7f062e80-f6c2-11e8-92bb-70653f5571d2.jpg)

>/src/app/test-content-child/test-content-child.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'test-content-child',
  templateUrl: './test-content-child.component.html',
  styleUrls: ['./test-content-child.component.scss']
})
export class TestContentChildComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }
  
}

```
>/src/app/test-content-child/test-content-child.component.html

```html
<div class="panel panel-primary">
    <div class="panel-heading">父组件</div>
    <div class="panel-body">
        <child-one>
          <h3>投影的标题</h3>
          <p>投影的底部</p>
          <child-two></child-two>
          <child-two></child-two>
          <child-two></child-two>
          <child-two></child-two>
          <child-two></child-two>
          <child-two></child-two>
          <child-two></child-two>
          <child-two></child-two>
        </child-one>
    </div>
  </div>
```


>/src/app/test-content-child/child-one/child-one.component.ts

```ts
import { Component, ContentChild, ContentChildren, ElementRef, OnInit, QueryList } from '@angular/core';
import { ChildTwoComponent } from '../child-two/child-two.component';

@Component({
  selector: 'child-one',
  templateUrl: './child-one.component.html',
  styleUrls: ['./child-one.component.scss']
})
export class ChildOneComponent implements OnInit {
  // @ContentChild(ChildTwoComponent)
  // childTwo:ChildTwoComponent;
  @ContentChildren(ChildTwoComponent)
  childrenTwo: QueryList<ChildTwoComponent>;

  constructor() { }

  ngOnInit() {

  }

  ngAfterContentInit(): void {
    // console.log(this.childTwo);
    this.childrenTwo.forEach((item) => {
      console.log(item);
    });
  }
}

```
>/src/app/test-content-child/child-one/child-one.component.html

```html
<div class="panel panel-primary">
  <div class="panel-heading">
    <ng-content select="h3"></ng-content>
  </div>
  <div class="panel-body">
    <ng-content select="child-two"></ng-content>
  </div>
  <div class="panel-footer">
    <ng-content select="p"></ng-content>
  </div>
</div>
```

>/src/app/test-content-child/child-two/child-two.component.ts

```ts
import { Component, OnInit, EventEmitter } from '@angular/core';

@Component({
  selector: 'child-two',
  templateUrl: './child-two.component.html',
  styleUrls: ['./child-two.component.scss']
})
export class ChildTwoComponent implements OnInit {
  constructor() { }

  ngOnInit() {
  }

  public sayHello(): void {
    console.log("Hello 大漠穷秋!");
  }
}

```
>/src/app/test-content-child/child-two/child-two.component.html

```html
<div class="panel panel-primary">
    <div class="panel-heading">被投影的子组件</div>
    <div class="panel-body">
    </div>
  </div>
```