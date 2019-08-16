#### dynamic config

>npm install cross-env --save-dev

>./package.json

```JSON
{
  "name": "1",
  "version": "0.0.1",
  "description": "dynamic config",
  "main": "main.js",
  "scripts": {
    "dev": "cross-env NODE_ENV=development TYPE=typeA node main.js",
    "build": "cross-env NODE_ENV=production TYPE=typeA node main.js",
    "clean": "find ./dist -maxdepth 1 -not -name 'project.config.json' -not -name 'dist' | xargs rm -rf",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "王树贤 <airmusic@msn.com>",
  "license": "MIT",
  "devDependencies": {
    "cross-env": "^5.1.6"
  }
}

```
<hr>

>./main.js

```javascript
const config = require('./config')

console.log(config)

console.log('env.NODE_ENV in main.js::', process.env.NODE_ENV)
console.log('env.TYPE in main.js::', process.env.TYPE)

```
<hr>

>./config/index.js

```javascript
const defaultConfig = require('./default.json')

console.log(defaultConfig)
// node config/index.js
// { type: 'default' }

const myType = process.env.TYPE

module.exports = (() => {
  console.log('fn')
  if (typeof defaultConfig === 'object') {
    const config = Object.assign(defaultConfig, {num: 123}, {myType})
    return config
  } else {
    console.log('defaultConfig is not json')
  }
})()

console.log('env.NODE_ENV in index.js::', process.env.NODE_ENV)

```
<hr>

>./config/default.json

```JSON
{
  "type": "default"
}
```
