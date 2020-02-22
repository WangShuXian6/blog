#### node 上传资源到阿里 oss

#### 基础版本

>https://github.com/ali-sdk/ali-oss?spm=a2c4g.11186623.2.13.144210d5HKi5cS#summary

```bash
npm install ali-oss --save
```
```package.json
{
  "name": "oss",
  "version": "1.0.0",
  "description": "",
  "main": "oss.js",
  "scripts": {
    "test": "node ./oss.js",
    "build": "node ./oss.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "ali-oss": "^6.1.0"
  }
}

```
```js
let OSS = require('ali-oss');

let client = new OSS({
    region: 'oss-cn-beijing',
    // 云账号AccessKey有所有API访问权限，建议遵循阿里云安全最佳实践，部署在服务端使用RAM子账号或STS，部署在客户端使用STS。
    accessKeyId: 'xxx',
    accessKeySecret: 'xxx',
    bucket: 'hype'
});

async function put() {
    try {
        console.log('开始上传资源到 oss cdn');
        let result = await client.put('oss_test/2.jpeg', './2.jpeg');
        console.log('上传成功-', result);
    } catch (e) {
        console.log('上传失败-', e);
    }
}

put()

```
***

#### 自动搜索文件版本
![oss](https://user-images.githubusercontent.com/30850497/53228639-c21ddf80-36bc-11e9-928d-a7c1b0024bfd.jpg)

```bash
npm install ali-oss --save
npm install glob mime --save-dev
npm i cross-env --save-dev
```

```package.json
{
  "name": "oss",
  "version": "1.0.0",
  "description": "",
  "main": "oss.js",
  "scripts": {
    "test": "node ./oss.js",
    "build": "cross-env NODE_APP_DIR=oss_test node ./app/ossAll.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {},
  "devDependencies": {
    "ali-oss": "^6.1.0",
    "cross-env": "^5.2.0",
    "glob": "^7.1.3",
    "mime": "^2.4.0"
  }
}
```
>./app/ossApp.js

```javascript
let OSS = require('ali-oss');

const options = {
    scope: 'source', // 空间对象名称
    bucket: 'hype',
    appDir: process.env.NODE_APP_DIR
}

let client = new OSS({
    region: 'oss-cn-beijing',
    // 云账号AccessKey有所有API访问权限，建议遵循阿里云安全最佳实践，部署在服务端使用RAM子账号或STS，部署在客户端使用STS。
    accessKeyId: 'xxx',
    accessKeySecret: 'xxx',
    bucket: 'hype'
});

const glob = require('glob')
const mime = require('mime')
const path = require('path')

const isWindow = /^win/.test(process.platform)

let pre = path.resolve(__dirname, '../dist/') + (isWindow ? '\\' : '')

const files = glob.sync(
    `${path.join(
        __dirname,
        '../dist/**/*.?(js|css|map|png|jpg|svg|woff|woff2|ttf|eot)'
    )}`
)
pre = pre.replace(/\\/g, '/')

async function uploadFileCDN(files) {
    files.map(async file => {
        const key = getFileKey(pre, file)
        const extname = path.extname(file)
        const mimeName = mime.getType(extname)
        try {
            await client.put(`${options.appDir}/${key}`, file);
            console.log(`上传成功 key: ${key}`)
        } catch (err) {
            console.log('error', err)
        }
    })
}

function getFileKey(pre, file) {
    if (file.indexOf(pre) > -1) {
        const key = file.split(pre)[1]
        return key.startsWith('/') ? key.substring(1) : key
    }
    return file
}

(async () => {
    console.time('上传文件到cdn')
    await uploadFileCDN(files)
    console.timeEnd('上传文件到cdn')
})()

```