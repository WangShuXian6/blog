### 小程序UI组件库
### MinUI

#### Min 结合 Wepy
>安装 Min

```bash
$ sudo npm install -g @mindev/min-cli
```
>min -v 命令查看 min 的版本号

#### 安装 MinUI 组件
>新建 min.config.json配置文件

>Min 的执行依赖于配置文件，在项目的根目录下新建 min.config.json 文件。由于 wepy 要求在微信开发者工具中关闭 es6=>es5 功能，因此在 min.config.json 中需配置 babel，用于在 min 编译组件之后生成 es5 代码。babel 的配置如下所示：

```json
{
  "compilers": {
    "babel": {
      "sourceMaps": "inline",
      "presets": [
        "env"
      ],
      "plugins": [
        "syntax-export-extensions",
        "transform-class-properties",
        "transform-decorators-legacy",
        "transform-export-extensions"
      ]
    }
  }
}
```
>安装 wxc-toast 组件

```bash
$ min install @minui/wxc-toast
// npm install @minui/wxc-progress --save
```
>在 wpy 文件中使用 wxc-toast

>在 config 字段中配置 usingComponents 字段，wxc-toast 组件的路径是 dist 目录下 pages/index/index.js 相对于 wxc-toast 组件的路径。

```vue
<!-- index.wpy -->
<style lang="less">
</style>
<template>
  <view class="container">
    <wxc-toast
        class="J_toast"
        text="Hello World"></wxc-toast>
    <button bindtap="showToast">调用 show() 方法显示</button>
  </view>
</template>

<script>
  import wepy from 'wepy'
  import testMixin from '../mixins/test'

  export default class Index extends wepy.page {
    config = {
      navigationBarTitleText: 'test',
      usingComponents: {
        'wxc-toast': '../../packages/@minui/wxc-toast/dist/index'
      }
    }
    components = {}

    mixins = [testMixin]

    data = {}

    computed = {}

    methods = {
      showToast() {
        let $toast = this.$wxpage.selectComponent('.J_toast')
        $toast && $toast.show()
      }
    }

    events = {}

    onLoad() {}
  }
</script>
```
>该示例中，调用了 wxc-toast 组件的 show() 方法。

>值得注意的是，我们不能直接用 this.selectComponent 来获取 toast 的实例，因为 wepy 的 Page 是内部封装过的，没有 selectComponent 方法，用 this.$wxpage 可获取页面的真实 Page 对象实例，该实例中有selectComponent方法。

>对于暴露出组件方法的一些组件，比如 loading, dialog等组件，在 wepy 中都应通过这种方法来获取组件实例。