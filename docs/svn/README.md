#### svn
##### 检出项目
```bash
svn checkout svn://192.168.1.1/pro/domain
```

##### 更新项目
>如果后面没有目录，默认将当前目录以及子目录下的所有文件都更新到最新版本

```bash
svn update
```

##### 配置全局忽略文件
```bash
svn propset svn:ignore -R -F .svnignore .
```

##### .svnignore 示例
```bash
dist
node_modules
.idea
.wepycache
.DS_Store
*.wpy___jb_tmp___
```

##### 查看状态
```bash
svn status
svn status --no-ignore
```

##### 递归添加当前目录下的所有新文件
```bash
svn add . --force
```

##### 递归添加当前目录下的所有新文件,包括已忽略
```bash
svn add . --no-ignore --force
```

##### 提交代码
```bash
svn commit -m "提交描述"
```
***

>http://wiki.jikexueyuan.com/project/svn/basic-concepts.html     什么是版本控制系统(VCS) - SVN 教程 - 极客学院Wiki
