#### npm发布包

#### 注册npm账户
>https://npmjs.com

> - 全名：
> - 邮箱：
> - 用户名：重要！发布scoped包时会用到
> - 密码：
***
#### 全局安装nrm
>nrm是npm仓库管理的软件，可用于npm仓库的快速切换

```bash
npm i nrm -g
```

>nrm 常用命令：

```bash
 nrm //展示nrm可用命令
 nrm ls //列出已经配置的所有仓库
 nrm test //测试所有仓库的响应时间
 nrm add <registry> <url> //新增仓库
 nrm use <registry> //切换仓库
```
***
#### 发布包
>package

>npm官方建议规范的包至少包含：

> - package.json（包的基本信息）
> - README.md（文档）
> - index.js （入口文件）

>团体账户下的包发布流程和个人账户差别不大

***
#### 发布unscoped包

>创建工程文件夹

```bash
mkdir test && cd test
```
>创建package.json

>本次演示的包的入口文件是index.js，请务必确保package.json中字段main对应的值是“index.js”。

```bash
npm init
```

>创建README.md

>创建index.js

```javascript
module.exports = {
    printMsg() {
        console.log('just test CommonJS for npm')
    }
}
```

>目录结构：

```
└── test
    ├── README.md
    ├── index.js
    └── package.json
```
***
#### 登录
```bash
npm adduser
```
>输入：

> - 用户名
> - 密码
> - 邮箱[公开展示]
***

#### 用nrm切换到npm仓库，执行命令nrm use npm
```bash
nrm use npm
```
***
#### 发布
```bash
npm publish
```

***

***

#### 发布scoped包
>@user/package

#### 加作用域
>重新生成package.json,只是会给包增加了作用域

>@符号后面的是你注册npm账户时的username，如果不记得可以通过npm whoami查询。

```bash
npm init --scope=@username -y
```

***
#### 公共发布
>npm publish 命令执行，默认是进行私有发布

>https://docs.npmjs.com/cli/publish.html

```bash
npm publish --access public
```