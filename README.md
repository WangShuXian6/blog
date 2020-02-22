# FE-BLOG

#### 博客内容详见  Issues 或 [github blog][1] 或 [gitlab blog][2]
[1]:https://wangshuxian6.github.io/blog/
[2]:https://wangshuxian6.gitlab.io/blog/

>开发环境开启服务器

```bash
npm i docsify-cli -g
docsify serve docs
```


>env: ‘node\r’: No such file or directory
```

You can simply cd to /usr/local/lib/node_modules/docsify-cli/bin
and open docsify with vim
sudo vim docsify
then you can fix the line endings of the file with
:set ff=unix
then save and quit
:wq
and everything should be fine!
```