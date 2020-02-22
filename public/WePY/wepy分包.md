#### wepy分包
>截止2018-7-23 无分包：小程序限制大小为2MB。

>有分包：主包限制2MB,单个分包限制2MB,整个小程序所有分包大小不超过 8M.

>在src下建立分包目录

>src/packageTest/pages/pTest.wpy

>app.js配置的config字段为

```json
config = {
      pages: [
        'pages/test'
      ],
      subPackages:[
        {
          root:'packageTest',
          pages:[
            'pages/pTest'
          ]
        }
      ],
      window: {
        backgroundTextStyle: 'light',
        navigationBarBackgroundColor: '#fff',
        navigationBarTitleText: 'WeChat',
        navigationBarTextStyle: 'black'
      }
    }
```
>主包内点击按钮跳转到分包内的方法

```javascript
navToPack(){
        wx.navigateTo({
          url:'../packageTest/pages/pTest' // 相对路径
          // url:'/packageTest/pages/pTest' // 绝对路径
        })
      }
```

#### 小程序使用节流函数
```javascript
methods = {
  handleLeftmove: debounce((key, e) => {
    
  }, 500, false)
```

#### tabBar
>app.wpy

>注意：pages数组的第一项必须是tabBar的list数组的一员

```javascript
config = {
      pages: [
        'pages/index',
        'pages/get',
        'pages/tab',
        'pages/pageA',
        'pages/pageB'

      ],
      window: {
        backgroundTextStyle: 'light',
        navigationBarBackgroundColor: '#fff',
        navigationBarTitleText: 'WeChat',
        navigationBarTextStyle: 'black'
      },
      tabBar: {
        color: '#000',
        selectedColor: '#ff4777',
        backgroundColor: '#fff',
        borderStyle: 'white',
        position: 'bottom',
        list: [
          {
            pagePath: 'pages/index',
            text: '第0个',
            iconPath: './icon/a.jpg',
            selectedIconPath: './icon/s.jpg'
          },
          {
            pagePath: 'pages/pageA',
            text: '第一个',
            iconPath: './icon/a.jpg',
            selectedIconPath: './icon/s.jpg'
          },
          {
            pagePath: 'pages/pageB',
            text: '第二个',
            iconPath: './icon/b.jpg',
            selectedIconPath: './icon/s.jpg'
          }
        ]
      }
    }
```

#### watch 监听器
```typescript
import wepy from 'wepy'
import {connect} from 'wepy-redux'

@connect({}, {})

export default class ShippingAddress extends wepy.component {

  components = {}

  props = {
    info: {
      type: Object,
      default: {
        distance: '',
        title: '',
        detail: '',
        name: '',
        phone: ''
      }
    }
  }

  data = {
    hasAddress: false
  }

  events = {}

  watch = {
    info(newValue, oldValue) {
      if (newValue.title) {
        this.hasAddress = true
      } else {
        this.hasAddress = false
      }
      this.$apply()
    }
  }

  methods = {
    toggleAddress(toggleAddress) {
      this.toggleAddress = toggleAddress
    }
  }
}
```

#### wepy 重的页面跳转
```javascript
// 跳转到指定页面，可以反回
  goPageCanBack = (url, params = {}) => {
    if (!url) {
      wx.navigateTo({
        url: '/pages/error'
      })
    } else if (typeof url !== 'string') {
      console.warn('页面地址应为字符串格式')
      return false
    } else if (params && !isEmptyObject(params)) {
      const paramsString = buildParamsString(params)
      wx.navigateTo({
        url: `${url}?${paramsString}`
      })
    } else if (!params || isEmptyObject(params)) {
      wx.navigateTo({
        url: url
      })
    }
  }

  // 跳转到指定页面，不可以反回
  goPage = (url, params = {}) => {
    if (!url) {
      wx.redirectTo({
        url: '/pages/error'
      })
    } else if (typeof url !== 'string') {
      console.warn('页面地址应为字符串格式')
      return false
    } else if (params && !isEmptyObject(params)) {
      const paramsString = buildParamsString(params)
      wx.redirectTo({
        url: `${url}?${paramsString}`
      })
    } else if (!params || isEmptyObject(params)) {
      wx.redirectTo({
        url: url
      })
    }
  }
```
```javascript
this.goPage('/packageRider/pages/rider', {source: PURCHASE})
this.goPageCanBack('error', {source: PURCHASE})
```

#### 父子组件通信

>父组件调用子组件方法

```

```

#### 在子组件内读取自身属性值 props
```vue
this.props // 无有效值
```

#### 页面内发起微信分享

>注意：分享路径为

>path: 'pages/index?member_id='+this.memberId

>不同于原生小程序路径

```xml
<view class="share-wrapper">
      <text class="air-long-button blue invite-button">立即邀请</text>
      <button open-type="share" class="share"></button>
</view>
```
```less
.air-long-button{
  display: flex;
  align-items: center;
  justify-content: center;
  width: @widthL3;
  height: @height;
  border-radius: @height;
  background-color: @white;
  box-shadow: @boxShadow;
  font-size: @mainSize;
  line-height: normal;
  color: @mainColor;
  border: 2rpx solid @borderL1;
}

.air-long-button.blue{
  background-color: @blue;
  border: 2rpx solid @blue;
}

&>.share-wrapper{
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      &>.share{
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        opacity: 0;
      }
    }
```
```typescript
methods = {
    /**
     * 用户点击右上角或页面分享按钮分享
     */
    onShareAppMessage: function () {
      return {
        title: this.title,
        path: 'pages/index?member_id='+this.memberId
      }
    }
  }
```

***

#### wepy 小程序 使用富文本

##### wepy-html
>https://github.com/beiliao-web-frontend/wepy-html