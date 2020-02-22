#### NUXT 使用 gh-pages

>这里，我先假设小伙伴们都可以正常执行 nuxt generate 并生成对应的 dist 目录。

>为了项目的并行开发，我们一般会在 .gitignore 文件里面将打包文件给忽略掉，但我们静态化页面的部署有需要用到 dist 目录下的所有打包文件。所以这里我们将使用 gh-pages 将打包文件发布到我们的 git 仓库

>安装 gh-pages

```bash
npm i gh-pages -D
```
>然后在 package.json 写入配置（当然你也可以新建文件执行发布）

```JSON
"scripts": {
  "deploy": "gh-pages -d dist"
}
```
>执行 npm run deploy ，然后你的 dist 目录就会发到你们仓库的 gh-pages 分支了