#### 用ES6编写的模块发布到NPM

>将es6文件保存在src目录中，并在npm的预发布目录中创建你的lib

>使用Babel6

>.npmignore

```
/src/
```

>.gitignore

```
/lib/
/node_modules/
```

>安装Babel

```bash
npm install --save-dev babel-core babel-cli babel-preset-es2015
```

>.babelrc

```.babelrc
{
  "presets": [
    "es2015"
  ]
}

```

>package.json

```package.json
{
  "name": "es6fornpm",
  "version": "2.0.0",
  "description": "",
  "main": "lib/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "./node_modules/.bin/babel ./src --out-dir ./lib"
  },
  "author": "",
  "license": "MIT",
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-core": "^6.26.3",
    "babel-preset-es2015": "^6.24.1"
  }
}
```

>./src/index.js

```javascript

export default class es6ForNpm {
    constructor() {
        console.log('just test ES6 for npm')
    }
}

```

>编译后发布到npm

```bash
npm run build
npm publish
```
***

#### 使用
>安装依赖

>index.js

```javascript
import es6ForNpm from 'es6ForNpm'
new es6ForNpm()
```
>编译后运行

***
>https://github.com/kriasoft/babel-starter-kit

>预配置好的发布NPM包的样板文件

***
>https://github.com/Hotell/typescript-lib-starter

>>预配置好的发布NPM包的样板文件 typescript 语法
>已知错误，初始化项目后需将初始化前的 CHANGELOG.md 文件复制回来