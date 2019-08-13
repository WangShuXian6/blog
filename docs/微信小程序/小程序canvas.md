#### 小程序 canvas
#### arc 圆形

>circle.wxml

```xml
<!--components/circle/circle.wxml-->
<view class="container">
    <canvas class="canvas" canvas-id="myCanvas" width="50" height="50"/>
</view>
```

>circle.wxss

```less
@import (reference) "../../style/color";

.canvas{
  width: 100rpx;
  height: 100rpx;
}
```


>circle.js

```javascript
// components/circle/circle.js
Component({
  multipleSlots: true, // 在组件定义时的选项中启用多slot支持
  /**
   * 组件的属性列表
   */
  properties: {},

  /**
   * 组件的初始数据
   */
  data: {},

  /**
   * 组件的方法列表
   */
  methods: {
    drawCircle() {
      const ctx = wx.createCanvasContext('myCanvas', this)
      ctx.arc(25, 25, 25, 0, 2 * Math.PI)
      ctx.setFillStyle('#EEEEEE')
      ctx.fill()
      ctx.draw()
    }
  },

  ready() {
    this.drawCircle()
  },


})

```
***

#### 微信小程序 canvas 无法同时指定画布像素宽高和显示宽高
>如果使用rpx,会同时动态调整画布像素宽高和显示宽高


>固定画布像素宽高方法

>画布像素宽高为512 ，512

>画布显示宽高为 512*设备像素比

```
<Canvas
          style="width: 512px; height: 512px;"
          canvasId="myCanvasLut"
          disable-scroll
          className='myCanvasLut'
        />
```