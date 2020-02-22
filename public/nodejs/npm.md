#### npm

>npm-check 依赖更新

>https://segmentfault.com/a/1190000011085967

> npm install -g npm-check //全局安装。项目下安装可自行选择

> npm install npm-check  --save-dev  //项目下安装，项目根目录执行

> npm install --save-dev cross-env

>package.json

```json
"scripts": {
    "start": "webpack-dev-server",
    "nc-u":"cross-env NPM_CHECK_INSTALLER=cnpm npm-check -u"
  },
  "devDependencies": {
    "cross-env": "^5.0.5"
  }
```
>npm-check指令列表

```bash
-u, --update       显示一个交互式UI，用于选择要更新的模块，并自动更新"package.json"内包版本号信息
-g, --global       检查全局下的包
-s, --skip-unused  忽略对未使用包的更新检查
-p, --production   忽略对"devDependencies"下的包的检查
-d, --dev-only     忽略对"dependencies"下的包的检查
-i, --ignore       忽略对指定包的检查.
-E, --save-exact   将确切的包版本存至"package.json"(注意，此命令将存储'x.y.z'而不是'^x.y.z')
```
<hr>

***

#### 官方解决所有 npm 全局安装权限问题
>https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally

>If you see an EACCES error when you try to install a package globally, you can either:
 
> - Reinstall npm with a node version manager (recommended),

or
> - Manually change npm’s default directory

>// 当前用户根目录下

```bash
rm -rf .node-gyp
```
```bash
// 重装nodejs
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
NPM_CONFIG_PREFIX=~/.npm-global
```
添加环境变量
```bash
cd ~
vi .bashrc
// 输入
export PATH=~/.npm-global/bin:$PATH
// 保存退出

// 加载新配置
source ./.bashrc
echo "[ -r ~/.bashrc ] && source ~/.bashrc" >> .bash_profile
```

***
#### 禁用 package-lock.json

>*nix users may use:

```bash
echo 'package-lock=false' >> .npmrc
echo 'package-lock.json' >> .gitignore
```

>Disabling package-lock.json Globally

```bash
npm config set package-lock false
```

>Installing without creating the lock (one time)

```bash
rm -f package-lock.json && \
npm install lodash --save && \
rm -f package-lock.json
```

>修复权限

```bash
sudo chown -R $USER:$GROUP ~/.npm
sudo chown -R $USER:$GROUP ~/.config
```

>
```bash
sudo npm install --unsafe-perm --verbose -g sails
```
***
