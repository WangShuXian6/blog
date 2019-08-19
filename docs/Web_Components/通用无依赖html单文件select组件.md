#### 使用 web component 构建一个通用无依赖 html 单文件 select 组件

>效果

![WeChat1e523a42cfe99bd2599505b2e1f6edf0](https://user-images.githubusercontent.com/30850497/62176556-98a46680-b374-11e9-858a-992d6132b084.png)


>体验

[web component select][1]

>web components polyfill 兼容旧版本浏览器的支持插件

>https://www.webcomponents.org/polyfills

>源码

```javascript
(function () {
  const selectListDemo = [
    {name: 'test1', value: 1},
    {name: 'test2', value: 2},
    {name: 'test3', value: 3}
  ]

  class MidociSelect extends HTMLElement {
    static get observedAttributes() {
      return ['acitve-title', 'active-sub-title']
    }

    constructor() {
      super()
      this.attachShadow({mode: 'open'})
      this.shadowRoot.innerHTML = `
        <style>
          :host{
            --themeColor:rgb(24,144,255);
            box-sizing: border-box;
            font-size: 14px;
            --borderColor:#eee;
          }
          
          .wrapper{
            position: relative;
            display: inline-flex;
            align-items: center;
            padding-left: 10px;
            width: 95px;
            height: 36px;
            border: 1px solid var(--borderColor);
            color: #333;
            border-radius: 2px;
            user-select: none;
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
            outline:none
          }
          
          .wrapper:hover{
            border: 1px solid var(--themeColor);
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          .title{
            
          }
          
          .arrow-out{
            position: absolute;
            right: 12px;
            top: 50%;
            transform: translateY(0px) rotateX(0deg);
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          .wrapper.flip>.arrow-out{
            transform: translateY(-3px) rotateX(180deg);
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          .arrow{
            display: flex;
            width: 6px;
            height:6px;
            border: none;
            border-left: 1px solid #333;
            border-bottom: 1px solid #333;
            transform: translateY(-50%) rotateZ(-45deg);
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          .wrapper:hover .arrow{
            border-left: 1px solid var(--themeColor);
            border-bottom: 1px solid var(--themeColor);
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          
          
          .list{
            z-index: 100;
            position: absolute;
            top: 130%;
            left: 0;
            background-color: #fff;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            visibility: hidden;
            min-width: 100%;
            border-radius: 3px;
            transform: scale(0);
            transform-origin: top;
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          .wrapper.flip>.list{
          visibility: visible;
            transform: scale(1);
            transition: .3s cubic-bezier(.12, .4, .29, 1.46);
          }
          
          .item{
            display: flex;
            align-items: center;
            padding-left: 10px;
            width: 95px;
            height: 36px;
            color: #333;
            border-radius: 2px;
            user-select: none;
            background-color: #fff;
            transition: background-color .3s ease-in-out;
          }
          
          .item:hover{
            background-color: rgba(24,144,255,0.1);
            transition: background-color .3s ease-in-out;
          }
        </style>
        
        <div class="wrapper" tabindex="1">
          <span class="title">1</span>
          <span class="arrow-out">
            <span class="arrow"></span>
          </span>
          <div class="list" >
            <div class="item">1</div>
            <div class="item">2</div>
            <div class="item">3</div>
            <div class="item">4</div>
          </div>
        </div>
      `
      this._wrapperDom = null
      this._listDom = null
      this._titleDom = null
      this._list = []
      this._arrowFlip = false
      this._value = null
      this._name = null
    }

    connectedCallback() {
      this._wrapperDom = this.shadowRoot.querySelector('.wrapper')
      this._listDom = this.shadowRoot.querySelector('.list')
      this._titleDom = this.shadowRoot.querySelector('.title')
      this.initEvent()
      this.list = selectListDemo
    }

    disconnectedCallback() {
      this._wrapperDom.removeEventListener('click', this.flipArrow.bind(this))
      this._wrapperDom.removeEventListener('blur', this.blurWrapper.bind(this))

      this.shadowRoot.querySelectorAll('.item')
        .forEach((item, index) => {
          item.removeEventListener('click', this.change.bind(this, index))
        })
    }

    attributeChangedCallback(attr, oldVal, newVal) {
      // const attribute = attr.toLowerCase()
      // if (attribute === 'descriptions') {
      //   console.log(1)
      //   this.render(newVal)
      // }
    }

    set list(list) {
      if (!this.shadowRoot) return
      this._list = list
      this.render(list)
    }

    get list() {
      return this._list
    }

    set value(value) {
      this._value = value
    }

    get value() {
      return this._value
    }

    set name(name) {
      this._name = name
    }

    get name() {
      return this._name
    }

    initEvent() {
      this.initArrowEvent()
      this.blurWrapper()
    }

    initArrowEvent() {
      this._wrapperDom.addEventListener('click', this.flipArrow.bind(this))
    }

    initChangeEvent() {
      this.shadowRoot.querySelectorAll('.item')
        .forEach((item, index) => {
          item.addEventListener('click', this.change.bind(this, index))
        })
    }

    change(index) {
      this.changeTitle(this._list, index)

      let changeInfo = {
        detail: {
          value: this._value,
          name: this._name
        },
        bubbles: true
      }
      let changeEvent = new CustomEvent('change', changeInfo)
      this.dispatchEvent(changeEvent)
    }

    changeTitle(list, index) {
      this._value = list[index].value
      this._name = list[index].name
      this._titleDom.innerText = this._name
    }

    flipArrow() {
      if (!this._arrowFlip) {
        this.showList()
      } else {
        this.hideList()
      }
    }

    showList() {
      this._arrowFlip = true
      this._wrapperDom.classList = 'wrapper flip'
    }

    hideList() {
      this._arrowFlip = false
      this._wrapperDom.classList = 'wrapper'
    }

    blurWrapper() {
      this._wrapperDom.addEventListener('blur', (event) => {
        event.stopPropagation()
        this.hideList()
      })
    }

    render(list) {
      if (!list instanceof Array) return
      let listString = ''
      list.forEach((item) => {
        listString += `
          <div class="item" data-value="${item.value}">${item.name}</div>
        `
      })
      this._listDom.innerHTML = listString
      this.changeTitle(list, 0)
      this.initChangeEvent()
    }
  }

  const FrozenMidociSelect = Object.freeze(MidociSelect);
  customElements.define('midoci-select', FrozenMidociSelect);
})()
```

>注意：如果父元素高度太低，需要关闭父元素的 overflow 属性，否则会遮盖 下拉列表

***
#### 使用

```html
<script type="module" async>
    import './MidociSelect.js'
</script>

<midoci-select></midoci-select>

<script>
    const list = [
        {name: '全平台', value: 1},
        {name: '东券', value: 2},
        {name: '京券', value: 3}
      ]

    window.onload=function(){
        document.querySelector('midoci-select').list=list
        
        console.log(document.querySelector('midoci-select').value)
        console.log(document.querySelector('midoci-select').name)
    
        document.querySelector('midoci-select').addEventListener('change', (event) => {
        console.log('选中的 value:', event.detail.value)
        console.log('选中的 name:', event.detail.name)
      })
    }
</script>
```
  [1]: https://codepen.io/WangShuXian6/pen/ymbMYe