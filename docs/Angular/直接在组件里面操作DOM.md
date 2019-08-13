#### 直接在组件里面操作 DOM

>test-component

>既然组件是指令的子类，那么指令里面能干的事儿组件应该都能干，可以在指令里面直接操作 DOM

>直接在组件里面来实现背景高亮效果，关键代码如下：

```ts
@Component({
  selector: 'test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.scss']
})
export class TestComponent implements OnInit {
  @Input() 
  highlightColor: string;

  private containerEl:any;

  constructor(private el: ElementRef) {

  }

  ngOnInit() {
  }

  ngAfterContentInit() {
    console.log(this.el.nativeElement);
    console.log(this.el.nativeElement.childNodes);
    console.log(this.el.nativeElement.childNodes[0]);
    console.log(this.el.nativeElement.innerHTML);

    this.containerEl=this.el.nativeElement.childNodes[0];
  }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.containerEl.style.backgroundColor = color;
  }
}
```

>组件的标签结构如下：

```html
<div class="my-container">
  鼠标移进来就会改变背景
</div>
```

>这个组件的使用方式如下：

```html
<div class="container">
    <test highlightColor="#F2DEDE"></test>
</div>
```

>直接在组件里面操作 DOM 是可以的，但是一旦把操作 DOM 的这部分逻辑放在组件里面，就没法再在其他标签上面使用了。

***
