#### @ViewChild & @ViewChildren

>viewchild

#### @ViewChild
>我们可以利用 @ViewChild 这个装饰器来操控直属的子组件。

```html
<div class="panel panel-primary">
  <div class="panel-heading">父组件</div>
  <div class="panel-body">
    <child-one></child-one>
  </div>
</div>
```
```ts
import { Component, OnInit, ViewChild, ViewChildren, QueryList } from '@angular/core';

@ViewChild(ChildOneComponent)
childOne:ChildOneComponent;

//在 ngAfterViewInit 这个钩子里面可以直接访问子组件
ngAfterViewInit():void{
    console.log(this.childOne);
    //用代码的方式订阅子组件上的事件
    this.childOne.helloEvent.subscribe((param)=>{
        console.log(this.childOne.title);
    });
}
```

#### @ViewChildren
```html
<div class="panel panel-primary">
  <div class="panel-heading">父组件</div>
  <div class="panel-body">
    <child-one></child-one>
    <child-one></child-one>
    <child-one></child-one>
    <child-one></child-one>
    <child-one></child-one>
  </div>
</div>
```
```ts
import { Component, OnInit, ViewChild, ViewChildren, QueryList } from '@angular/core';

@ViewChildren(ChildOneComponent)
children:QueryList<ChildOneComponent>;

ngAfterViewInit():void{
    this.children.forEach((item)=>{
        // console.log(item);
        //动态监听子组件的事件
        item.helloEvent.subscribe((data)=>{
        console.log(data);
        });
    });
}
```
***
![viewchild](https://user-images.githubusercontent.com/30850497/49345730-00aa8c00-f6c4-11e8-87c5-f8c9259af618.jpg)

>/src/app/test-view-child/test-view-child.component.ts

```ts
import { Component, OnInit, ViewChild, ViewChildren, QueryList } from '@angular/core';
import { ChildOneComponent } from './child-one/child-one.component';

@Component({
  selector: 'test-view-child',
  templateUrl: './test-view-child.component.html',
  styleUrls: ['./test-view-child.component.scss']
})
export class TestViewChildComponent implements OnInit {
  // @ViewChild(ChildOneComponent)
  // childOne:ChildOneComponent;
  @ViewChildren(ChildOneComponent)
  children: QueryList<ChildOneComponent>;

  constructor() { }

  ngOnInit() {
  }

  ngAfterViewInit(): void {
    // console.log(this.childOne);
    // this.childOne.helloEvent.subscribe((param)=>{
    //   console.log(this.childOne.title);
    // });
    this.children.forEach((item) => {
      // console.log(item);
      //动态监听子组件的事件
      item.helloEvent.subscribe((data) => {
        console.log(data);
      });
    });
  }
}

```
>/src/app/test-view-child/test-view-child.component.html

```html
<div class="panel panel-primary">
  <div class="panel-heading">父组件</div>
  <div class="panel-body">
    <child-one></child-one>
    <child-one></child-one>
    <child-one></child-one>
    <child-one></child-one>
    <child-one></child-one>
  </div>
</div>

<div class="panel" contenteditable="true">
</div>
```

>/src/app/test-view-child/child-one/child-one.component.ts

```ts
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'child-one',
  templateUrl: './child-one.component.html',
  styleUrls: ['./child-one.component.scss']
})
export class ChildOneComponent implements OnInit {
  public id: string;

  @Input()
  public title: string = "我是ChildOne";

  @Output()
  helloEvent: EventEmitter<string> = new EventEmitter<string>();

  constructor() {
    this.id = "ID-" + Math.floor(Math.random() * 100000000);
  }

  ngOnInit() {
  }

  public sayHello(): void {
    this.helloEvent.emit(this.id);
  }
}

```
>/src/app/test-view-child/child-one/child-one.component.html

```html
<div class="panel panel-primary">
  <div class="panel-heading">{{title}}</div>
  <div class="panel-body">
      <button class="btn btn-success" (click)="sayHello()">触发事件</button>
  </div>
</div>
```