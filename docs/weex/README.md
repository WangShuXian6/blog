#### weex
>https://weex.apache.org/cn/

>https://github.com/apache/incubator-weex/

>npm install weex-toolkit -g

#### 使用async/await
```bash
npm install --save-dev babel-plugin-transform-runtime
```
>在 /babelrc 中加入如下内容

```JSON
"plugins": [
  [
    "transform-runtime",
    {
      "helpers": false,
      "polyfill": false,
      "regenerator": true,
      "moduleName": "babel-runtime"
    }
  ]
]
```
>示例

```vue
<template>
  <div class="wrapper">
    <image :src="logo" class="logo"/>
    <text class="greeting">The environment is ready!--</text>
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data () {
    return {
      logo: 'https://gw.alicdn.com/tfs/TB1yopEdgoQMeJjy1XaXXcSsFXa-640-302.png'
    }
  },
  async created () {
    await this.promising()
  },
  methods: {
    promising () {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          console.log(123456)
        }, 5000)
      })
    }
  }
}
</script>

<style scoped>
  .wrapper {
    justify-content: center;
    align-items: center;
  }

  .logo {
    width: 424px;
    height: 200px;
  }

  .greeting {
    text-align: center;
    margin-top: 70px;
    font-size: 50px;
    color: #41B883;
  }

  .message {
    margin: 30px;
    font-size: 32px;
    color: #727272;
  }
</style>

```

#### 初始化
>weex create awesome-app

#### 构建app
>https://github.com/weexteam/weex-pack/wiki/%E5%A6%82%E4%BD%95%E7%94%A8weexpack%E5%88%9B%E5%BB%BAweex%E9%A1%B9%E7%9B%AE%E5%B9%B6%E6%9E%84%E5%BB%BAapp

>npm install weexpack -g

>weex platform add ios

>weex platform add android

#### 安卓配置
>安装Android Studio

>https://developer.android.com/studio/

>http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

>新建一个 Android 项目，在第二步中选择 Android 5.1

>安装 Android 模拟器

>安装 Android build-tool 版本为 23.0.2

>配置Android_HOME环境变量和PATH

```bash
vim ~/.bash_profile
```
>然后在顶部加入

```javascript
export ANDROID_HOME=/Users/zl/Library/Android/sdk
  
export PATH=$PATH:$ANDROID_HOME/tools

export PATH=$PATH:$ANDROID_HOME/platform-tools
```
>source ~/.bash_profile

>关闭所有终端

#### ios 配置
>安装xcode 并打开一次

>安装cocoapods

>安装RVM 

```bash
curl -L http://get.rvm.io | bash -s stable
```
>更换Ruby镜像

```bash
1)检查当前镜像 gem sources -l
(2)移除当前镜像 gem sources --remove https://rubygems.org/ (具体看你上一步检查的结果)
(3)更换新的镜像 gem sources -a https://gems.ruby-china.com/
(4)检查新镜像是否安装成功 gem sources -l

```
>安装CocoaPods

```bash
sudo gem install -n /usr/local/bin cocoapods
```
>下载标准配置文件

```bash
pod setup
```
<hr/>

#### 运行
>weexpack run android

>weexpack run ios

#### 打包
>weexpack build ios

>weexpack build android

<hr/>

>每个单页中使用 vue 实例方法

```javascript
import Vue from 'vue'
```