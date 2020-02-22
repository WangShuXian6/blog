>虽然 this.props 由 Taro 本身设置以及 this.state 具有特殊的含义，但如果需要存储不用于视觉输出的东西，则可以手动向类中添加其他字段。

>如果你不在 render() 中使用某些东西，它就不应该在状态中。

***
>Taro.getEnv() 与 process.env.TARO_ENV

>https://github.com/NervJS/taro/issues/1080

```bash
Taro.getEnv() 返回 'WEAPP' | 'WEB' | 'RN' | 'SWAN' | 'ALIPAY'

process.env.TARO_ENV 返回 'weapp' | 'swan' | 'alipay' | 'h5' | 'rn'
```

>Taro.getEnv() 这个 API 设计的本意应该是用来运行时环境判断, 而 process.env.TARO_ENV 是在编译时替换成字符串

***

>横向滚动

![scrollx](https://user-images.githubusercontent.com/30850497/49996332-b382c000-ffc9-11e8-9869-adc683b4f6d3.jpg)
