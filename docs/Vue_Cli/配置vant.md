#### 配置 vant

>https://github.com/youzan/vant

>https://youzan.github.io/vant/

>安装 vant

```bash
npm i vant -S
```

>安装 babel-plugin-import

```bash
npm i babel-plugin-import -D
```

>使用 babel-plugin-import

>修改./babel.config.js 新增如下代码

```javascript
module.exports = {
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
}
```

>使用 vant

>./src/views/Home.vue

```vue
<template>
  <div class="home">
    <van-button round type="danger">圆形按钮</van-button>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import { Button, Cell } from 'vant'
import Vue from 'vue'
Vue.use(Button).use(Cell)

export default {
  name: 'home',
  components: {
    HelloWorld
  }
}
</script>

```

