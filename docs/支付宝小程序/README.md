#### 支付宝小程序

>>https://mini.open.alipay.com/channel/miniIndex.htm

#### 小程序跳转收藏有礼页面
>https://opensupport.alipay.com/support/knowledge/39202/201602386807?ant_source=zsearch

>https://spcenter.alipay.com/operation/mini/ops/dashboard

>手册 

>https://gw.alipayobjects.com/os/basement_prod/44fa9099-b5d9-43b1-a1fc-4f18c3d50bf7.pdf

```ts
my.navigateToMiniProgram({
    appId: '2018122562686742',//收藏有礼小程序的appid，固定值请勿修改 
    path: 'pages/index/index?originAppId=2017082508366123&newUserTemplate=20190130000000119123',//收藏有礼跳转地址和参数 
    success: (res) => {
        // 跳转成功 
        my.alert({ content: 'success' });
    },
    fail: (error) => {
        // 跳转失败 
        my.alert({ content: 'fail' });
    }
});
```
>originAppId 为发布收藏有礼活动的小程序 appid

>newUserTemplate 为常见的收藏有礼的活动 id [收藏有礼模版id]

![收藏有礼](https://user-images.githubusercontent.com/30850497/56269171-b03d4380-6125-11e9-8189-c076e7504609.jpg)

***

>小程序跳转分享有礼页面

>https://opensupport.alipay.com/support/knowledge/39202/201602386805?ant_source=zsearch

```ts
my.navigateToMiniProgram({
    appId: '2018121862587354',//分享有礼小程序的appid，固定值请勿修改 
    extraData: {
        shareTemplateId: 'V2MS20190130000000765123',//活动进行中的模板id，可以在运营中心的分享有礼列表里查看 
        appId: '2017082508366123'//对应的模板配置的小程序appid 
    },
    success: (res) => {
        my.alert({ content: 'success' });
    },
    fail: (res) => {
        my.alert({ content: 'fail' });
    }
});
```

#### 小程序智能客服

>智能客服组件的使用参考文档：https://docs.alipay.com/mini/component/contact-button，接入智能客服组件之前先看一遍接入文档，方便后续更好的开发。 
智能客服组件遇到问题如何解决：云服务相关问题，可以拨打云服务支持热线：4009951887 ； 
或到登录开放平台-开发者中心-小程序-云服务中提工单，直达链接：https://openhome.alipay.com/platform/cloudManage.htm?tab=ticket&gotoUrl=https%3A%2F%2Fticket.cloud.alipay.com%2Findex