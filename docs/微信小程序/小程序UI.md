#### 小程序 UI

***
#### 个人头像，姓名
```xml
<!--components/avatar/avatar.wxml-->
<view class="container">
    <open-data type="userAvatarUrl" class="user-avatar"></open-data>
    <open-data type="userNickName" class="name"></open-data>
</view>
```
>avatarColumn.less

```less
@import (reference) "../../style/color";

@height: 100rpx;
@fontSize: 26rpx;

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  height: 146rpx;
  & > .user-avatar {
    display: flex;
    width: @height;
    height: @height;
    border-radius: 50%;
    overflow: hidden;
  }
  & > .name{
    font-size: @fontSize;
    color: @white;
  }
}
```
>avatarColumn.wxss

```css
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  height: 146rpx;
}
.container > .user-avatar {
  display: flex;
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
  overflow: hidden;
}
.container > .name {
  font-size: 26rpx;
  color: #fff;
}
```
>avatarColumn.js

```javascript
// components/avatar/avatar.js
// 用户头像 答题页专用
Component({
  /**
   * 组件的属性列表
   */
  properties: {
  },

  /**
   * 组件的初始数据
   */
  data: {},

  /**
   * 组件的方法列表
   */
  methods: {}
})
```
>avatarColumn.json

```JSON
{
  "component": true,
  "usingComponents": {}
}
```
***
