#### 小程序API注意事项
>navigateToMiniProgram:fail 此接口已废弃，

>请使用 `<navigator target="miniProgram" open-type="navigateTo" />` 组件

<hr>

>领卡

```javascript
onShow(options){
      let info = options.query
      info.scene = options.scene
      ENV === 'development' && console.log('模版消息参数',info)
      ENV === 'development' && console.log('APP--options',options)
      // 更新扫码获取的桌台及门店id
      this.globalData.data = info
      this.checkHaveActiveMember(options)
    }

    checkHaveActiveMember(options){
      if(options.referrerInfo.appId==='wxeb490c6f9b154ef9'){
        ENV === 'development' && console.log('通过开卡小程序组件返回',options.referrerInfo.appId)
        ENV === 'development' && console.log('通过开卡小程序组件返回--options.referrerInfo.extraData',options.referrerInfo.extraData)
        if(options.referrerInfo.extraData){
          ENV === 'development' && console.log('通过开卡小程序组件返回-并且点击了 立即开卡 成为会员--',options.referrerInfo.appId)
          this.globalData.haveActiveMember=true
          this.globalData.extraData=options.referrerInfo.extraData
        }
      }
    }
```
> 强制刷新缓存页面

```javascript
async onShow(){
  this.onLoad()
}
```
<hr>

>网络请求

>method:

>需大写-有效值：OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT

>注意：没有PATCH

```javascript
wx.request(OBJECT)


```
<hr>

>获取用户信息方案介绍 [官方](https://developers.weixin.qq.com/blogdetail?action=get_post_info&lang=zh_CN&docid=c45683ebfa39ce8fe71def0631fad26b)

<hr/>

>对界面内容进行设置的 API 如wx.setNavigationBarTitle，请在onReady之后进行

<hr/>