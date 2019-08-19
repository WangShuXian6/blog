#### webview

#### 返回
>引入基础库

```html
<script src="https://appx/web-view.min.js"></script>
```
```ts
my.navigateTo({url: '/pages/list/index'})
```

***
#### webview 发送消息
```ts
// H5向小程序发送消息
      my.postMessage({'sendToMiniProgram': '0'});
```
#### webview 接收消息
```ts
my.onMessage = function(e) {
          console.log(e); //{'sendToWebView': '1'}
      }
```

#### 小程序
```ts
class Webview extends Component {
    config: Config = {
        navigationBarTitleText: '编辑'
    }

    constructor() {
        super(...arguments)
        this.state = {
            webViewContext:null
        }
    }

    componentDidMount() {
        this.setState({
            webViewContext : my.createWebViewContext('mywebview')
        })
    }

    componentWillReceiveProps(nextProps) {

    }

    componentWillUnmount() {
    }

    componentDidShow() {

    }

    componentDidHide() {
    }

    onMessage(e){
        console.log(e); //{'sendToMiniProgram': '0'}
        // 向H5发送消息
        this.state.webViewContext.postMessage({'sendToWebView': '1'});
    }

    render() {
        return (
            <WebView src='http://192.168.1.86:8081/index.html'
                     id='mywebview'
                     onMessage={this.onMessage}
            />
          
        )
    }
}
```