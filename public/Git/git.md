# git

### git 公用配置

>.gitattributes

```
# Auto detect text files and perform LF normalization
# git config --global core.autocrlf false
text=LF
text eol=lf
```
>.gitignore

>WePY

```
node_modules
dist
.DS_Store
```

### 忽略已在版本控制中的文件

>正确的做法应该是：git rm --cached logs/xx.log -r，
>然后更新 .gitignore 忽略掉目标文件，
>最后 git commit -m "We really don't want Git to track this anymore!"

***

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
### 版本控制-Issue
=======
>"问题"或"事务"

>Issue 是最通用的项目管理工具

>Issue 指的是一项待完成的工作

>每个 Issue 应该包含该问题的所有信息和历史

>Issue 的原始功能是问题追踪和工单管理，后来不断扩展，逐渐演变成全功能的项目管理工具，还可以用于制定和实施软件的开发计划。

>commit message title, #1
>>这个提交会作为一个 comment ，出现在编号为1的 issue 记录中

>commit message title, fix #n
>>可以自动关闭第 n 个 issue
#### Issue 跟踪管理系统应该具有以下功能。

>项目管理 
>>指定 Issue 的优先级 
>>指定 Issue 所在的阶段 
>>分配负责 Issue 的处理人员 
>>制定日程 
>>监控进度，提供统计

>团队合作 
>>讨论 
>>邮件通知

>代码管理 
>>将 Issue 关联源码 
>>将 Issue 关联代码提交与合并

<hr>

#### Github Issues
>新建 Issue 配置

>- Assignees：人员
>- Labels：标签
>- Projects：项目
>- Milestone：里程碑

>Assignee 选择框用于从当前仓库的所有成员之中，指派某个 Issue 的处理人员。

>Labels 可以贴上标签，这样有利于分类管理和过滤查看

>Milestone "里程碑"，用作 Issue 的容器，相关 Issue 可以放在一个 Milestone 里面。常见的例子是不同的版本（version）和迭代（sprint），都可以做成 Milestone。可以指定到期时间

<hr>

#### Labels
>每个 Issue 至少应该有两个 Label ，一个表示性质，另一个表示优先级

>表示性质的 Label

>https://robinpowered.com/blog/best-practice-system-for-organizing-and-tagging-github-issues/
![label](https://user-images.githubusercontent.com/30850497/42730190-0629e980-8821-11e8-8b98-4e45e870baa9.png)


>表示优先级的 Label

>>高优先级（High）：对系统有重大影响，只有解决它之后，才能去完成其他任务。

>>普通优先级（Medium）：对系统的某个部分有影响，用户的一部分操作会达不到预期效果。

>>低优先级（Low）：对系统的某个部分有影响，用户几乎感知不到。

>>微不足道（Trivial）：对系统的功能没有影响，通常是视觉效果不理想，比如字体和颜色不满意。

***

#### 全局视图
>访问 github.com/issues 这个网址，就可以打开全局视图

>- Created：你创建的 Issue
>- Assigned：分配给你的 Issue
>- Mentioned：提及你的 Issue

***
#### 看板
>主要用于项目的进度管理。所有需要完成的任务，都做成卡片，贴在一块白板上面，这就是看板。

>在仓库首页进入 Projects 面板
>点击 New Project 按钮，新建一个 Project，比如"2.0 版"
>点击 Add column 按钮，为该项目新建若干列
>最后，将 Issue 分配到对应的列，就新建成功了一个看板视图
>Issue 可以从一列拖到另一列，表示从一个阶段进入另一个阶段

>- Todo （待安排）
>- Plan （计划）
>- Develop/Doing （开发）
>- Test （测试）
>- Deploy （部署）
>- Done （已完成）

>许多第三方工具可以增强 Github 的看板功能，最著名的是 Zenhub
>https://github.com/marketplace/zenhub
***
###Close Issue
>如果一个处于Open状态的Issue已经处理完毕，只要在该提交中以下列任意一种格式描述提交信息，对应的Issue就会被Close。
>+ fix #24
>+ fixes #24
>+ fixed #24
>+ close #24
>+ closes #24
>+ closed #24
>+ resolve #24
>+ resolves #24
>+ resolved #24

>利用这个方法，每次提交并Push之后，就不必再大费周章地到GitHub的Issue中寻找相应的Issue手动Close，省去了不少麻烦。像这样，只要按照特定的格式描述提交信息，Github就会自动识别并处理，很多GitHub之外的BTS也实现了这一功能。

***
#### 本月要做的任务
>这样一来，这段文字就会呗标记成复选框列表的样式。这个复选框列表可以直接勾选或者取消，不必打开Issue的编辑页面重新编辑，十分方便。

```bash
- [x] 完成部署工具的设置
- [ ] 博客文章更新
- [ ] 实现抽奖功能
```
>效果
- [x] 完成部署工具的设置
- [ ] 博客文章更新
- [ ] 实现抽奖功能
***
>http://www.ruanyifeng.com/blog/2017/08/issue.html

### Label Groups

>We group labels by color, according to broad themes. Labels are consistent across repositories, except for a few language specific topics. This makes switching between projects easy, since you don’t need domain expertise in order to write an issue. New team members can learn the system once, and use it everywhere.

>Platform 平台 #C0D4F1
>>If the repository covers multiple parts, this is how we designate where the issue lives. (i.e. iOS and Android for cross-platform tablet app).

>Problems 问题  #F61F28
>>Issues that make the product feel broken. High priority, especially if its present in production.

>Mindless 无影响 #FEF1C2
>>Converting measurements, reorganizing folder structure, and other necessary (but less impactful) tasks.

>Experience 体验 #FEC17A
>>Affect user’s comprehension, or overall enjoyment of the product. These can be both opportunities and “UX bugs”.

>Environment 环境 #F9D8C8
>>Server environment. With good QA, you’ll identify issues on test and staging deployments.

>Feedback 反馈 #CA357C
>>Requires further conversation to figure out the action steps. Most feature ideas start here.

>Improvements 改良 #63BFFC
>>Iterations on existing features or infrastructure. Generally these update speed, or improve the quality of results. Adding a new “Owner” field to an existing “Calendar” model in the API, for example.

>Additions 新功能 #92C85C
>>Brand new functionality. New pages, workflows, endpoints, etc.

>Pending 待定 #FAC92F
>>Taking action, but need a few things to happen first. A feature that needs dependencies merged, or a bug that needs further data.

>Inactive 闲置 #D2DAE1
>>No action needed or possible. The issue is either fixed, addressed better by other issues, or just out of product scope.

>>See a version of this in action on one of our public repos. Want to see what we’re building? Check out our product features.

![iphone 6-7-8 1](https://user-images.githubusercontent.com/30850497/42731213-41f61050-883b-11e8-8eaa-53ad4cd4e350.png)

>https://weihongyu.com/基于gitlab的工作流程设计/

### gitlab 默认标记

>bug 错误

>confirmed 批准

>critical 关键

>discussion 讨论

>documentation 文档

>enhancement 改善

>suggestion 建议

>support 支持

![gitlab-labels](https://user-images.githubusercontent.com/30850497/42731628-b02be54c-8843-11e8-9844-2d923e43669f.jpg)

### GitLab 软件开发阶段
>一般情况下，软件开发经过 10 个主要阶段；GitLab 为这 10 个阶段依次提供了解决方案：

>- IDEA： 每一个从点子开始的项目，通常来源于一次闲聊。在这个阶段，GitLab 集成了Mattermost。

>- ISSUE： 最有效的讨论一个点子的方法，就是为这个点子建立一个工单讨论。你的团队和你的合作伙伴可以在工单追踪器issue tracker中帮助你去提升这个点子

>- PLAN： 一旦讨论得到一致的同意，就是开始编码的时候了。但是等等！首先，我们需要优先考虑组织我们的工作流。对于此，我们可以使用工单看板Issue Board。
>- CODE： 现在，当一切准备就绪，我们可以开始写代码了。

>- COMMIT： 当我们为我们的初步成果欢呼的时候，我们就可以在版本控制下，提交代码到功能分支了。

>- TEST： 通过GitLab CI，我们可以运行脚本来构建和测试我们的应用。

>- REVIEW： 一旦脚本成功运行，我们测试和构建成功，我们就可以进行代码复审code review以及批准。

>- STAGING：： 现在是时候将我们的代码部署到演示环境来检查一下，看看是否一切就像我们预估的那样顺畅——或者我们可能仍然需要修改。

>- PRODUCTION： 当一切都如预期，就是部署到生产环境的时候了！

>- FEEDBACK： 现在是时候返回去看我们项目中需要提升的部分了。我们使用周期分析 Cycle Analytics来对当前项目中关键的部分进行的反馈

***
### GitLab flow
>GitLab flow是GitLab官方推荐的分支管理策略，Gitlab flow 的最大原则叫做”上游优先”（upsteam first），即只存在一个主分支master，它是所有其他分支的”上游”。只有上游分支采纳的代码变化，才能应用到其他分支。

#### 分支约定
>临时分支：在开发完成会被删除

>>功能分支 feature – 用于新功能的开发，建议以issue-feature-name命名

>>修复分支 fix – 用户bug的修复，建议以issue-fix-name命名

>固定分支

>>开发分支 master – 用于发布到测试环境，上游分支为 feature 和 fix，该分支为受保护分支

>>预发分支 pre-production – 用于发布到预发环境，上游分支为 master

>>正式分支 production – 用于发布到正式环境，上游分支为 pre-production

####  使用流程

>1- 克隆项目到本地

```bash
git clone git@example.com:project-name.git
```
>2- 检出分支

```bash
git checkout -b $issue-feature-name
```
>3- 提交并push到GitLab仓库

```bash
git commit -am "My feature is ready"
git push origin $issue-feature-name
```
>4- 运行GitLab CI

>5- 在GitLab上创建一个Merge Request

>6- 项目管理者进行代码审查，合并到master

>7- 运行第二次GitLab CI

>8- 进行产品测试

>9- 将master分支合并到pre-production

>10- 运行第三次GitLab CI

>11- 进行产品测试

>12- 将pre-production分支合并到prouction，并且为prouction打上tag，保持prouction与线上代码一致

***
### 版本发布
>版本发布适用于APP、小程序等有版本规划的项目。

#### 分支约定

>临时分支：在开发完成会被删除

>>功能分支 feature – 用于新功能的开发，建议以issue-feature-name命名

>>修复分支fix – 用户bug的修复，建议以issue-fix-name命名

>固定分支

>>开发分支 master – 用于发布到测试环境，上游分支为 feature 和 fix，该分支为受保护分支

>>发布分支 stable – 用于发布到预发环境，上游分支为 master，建议以version-stable命名，该分支要>>尽可能晚的创建，每次更新此分支都要更新一个小版本号

#### 使用流程
>1- 克隆项目到本地
```bash
git clone git@example.com:project-name.git
```
>2- 检出分支
```bash
git checkout -b $issue-feature-name
```
>3- 提交并push到GitLab仓库
```bash
git commit -am "My feature is ready"
git push origin $issue-feature-name
```
>4- 运行GitLab CI
>5- 在GitLab上创建一个Merge Request
>6- 项目管理者进行代码审查，合并到master
>7- 运行第二次GitLab CI
>8- 进行产品测试
>9- 将master分支合并到stable，如果是新版本则创建一个新的stable分支
>10- 为stable打上tag，并进行发布

### 分支命名规范

#1-feature-homePage
#2-fix-loadingError
#3-docs-app
#4-style-userPage
#5-refactor-user
#6-test-home
#7-chore-webpackUpdate
***
### 提交描述规范
fix home page error,#2
fix home page error,close #2
***
>在 GitLab 的官方分支结构中，将 <主版本号>-<次版本号>-stable 做为版本分支，该版本的修改都在该分支中。而每次发布，其<修订号> 都会递增，并且会将发布版本标记为 v<主版本号>.<次版本号>.<修订号> 标签(Tag)。

***

#### CONTRIBUTING_COMMIT  Commit规范
在对项目作出更改后，我们需要生成 commit 来记录自己的更改。以下是参照 Angular 对 commit 格式的规范：

##### (1) 格式

提交信息包括三个部分：`Header`，`Body` 和 `Footer`。

```
<Header>

<Body>

<Footer>
```

其中，Header 是必需的，Body 和 Footer 可以省略。

#### 1> Header

Header 部分只有一行，包括俩个字段：`type`（必需）和`subject`（必需）。

```
<type>: <subject>
```

**type**

type 用于说明 commit 的类别，可以使用如下类别：

- feat：新功能（feature）
- fix：修补bug
- doc：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

**subject**

subject 是 commit 目的的简短描述。

- 以动词开头，使用第一人称现在时，比如改变，而不是改变了。
- 结尾不加句号（。）

#### 2> Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

**注意：**应该说明代码变动的动机，以及与以前行为的对比。

##### 3> Footer

​	Footer 部分应该包含：(1)Breaking Changes;  (2)关闭 issue；

​	**Breaking Changes**：

​	如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。这种使用较少，了解即可。

​	**Issue 部分：**

- 通过 commit 关联 issue：

  如果当前提交信息关联了某个 issue，那么可以在 Footer 部分关联这个 issue：

  ```
  issue #2
  ```

- 通过 commit 关闭 issue，当提交到**默认分支**时，提交信息里可以使用 `fix/fixes/fixed` ， `close/closes/closed` 或者 `resolve/resolves/resolved`等关键词，后面再跟上 issue 号，这样就会关闭这个 issue：

```
Closes #1
```

​	注意，如果不是提交到默认分支，那么并不能关闭这个 issue，但是在这个 issue 下面会显示相关的信息表示曾经想要关闭这个 issue，当这个分支合并到默认分支时，就可以关闭这个 issue 了。

##### 4> 例子

下面是一个完整的例子：

```
feat: 添加了分享功能

给每篇文章添加了分享功能

- 添加分享到微信功能
- 添加分享到朋友圈功能

Issue #1, #2
Closes #1
```

***

### Contributing 规范

我们提倡您通过提 issue 和 pull request 方式来促进 WePY 的发展。


##### Acknowledgements

非常感谢以下几位贡献者对于 WePY 的做出的贡献：

- dlhandsome [awsomeduan@gmail.com](mailto:awsomeduan@gmail.com)
- dolymood [dolymood@gmail.com](mailto:dolymood@gmail.com)
- baisheng [baisheng@gmail.com](mailto:baisheng@gmail.com)

其中特别致谢 dlhandsome 提交的38个 commits, 对 WePY 做出了1,350增加和362处删减(截止02/28/18日)。

WePY 持续招募贡献者，即使是在 issue 中回答问题，或者做一些简单的 bugfix ，也会给 WePY 带来很大的帮助。

WePY 已开发近一年，在此感谢所有开发者对于 WePY 的喜欢和支持，希望你能够成为 WePY 的核心贡献者，加入 WePY ，共同打造一个更棒的小程序开发框架！🍾🎉

​                       

#### Issue 提交

##### 对于贡献者

在提 issue 前请确保满足一下条件：

- 必须是一个 bug 或者功能新增。
- 必须是 WePY 相关问题，原生小程序问题去[开发者论坛](https://developers.weixin.qq.com/)。
- 已经在 issue 中搜索过，并且没有找到相似的 issue 或者解决方案。
- 完善下面模板中的信息

如果已经满足以上条件，我们提供了 issue 的标准模版，请按照模板填写。

​             

####  Pull request

我们除了希望听到您的反馈和建议外，我们也希望您接受代码形式的直接帮助，对我们的 GitHub 发出 pull request 请求。

以下是具体步骤：

##### Fork仓库

点击 `Fork` 按钮，将需要参与的项目仓库 fork 到自己的 Github 中。

#### Clone 已 fork 项目

在自己的 github 中，找到 fork 下来的项目，git clone 到本地。

```bash
$ git clone git@github.com:<yourname>/wepy.git
```

##### 添加 WePY 仓库

将 fork 源仓库连接到本地仓库：

```bash
$ git remote add <name> <url>
# 例如：
$ git remote add wepy git@github.com:Tencent/wepy.git
```

##### 保持与 WePY 仓库的同步

更新上游仓库：

```bash
$ git pull --rebase <name> <branch>
# 等同于以下两条命令
$ git fetch <name> <branch>
$ git rebase <name>/<branch>
```

##### commit 信息提交

commit 信息请遵循[commit消息约定](./CONTRIBUTING_COMMIT.md)，以便可以自动生成 `CHANGELOG` 。具体格式请参考 commit 文档规范。



##### 开发调试代码

```bash
# Build code
$ npm run build

# Watch
$ npm run watch

# Run test cases
$ npm run test

# Useage
$ wepy build # Global wepy you installed by npm

$ wepy-dev build # Local wepy you compiled by your local repository

$ wepy-debug build # Debug local wepy using node --inspect
```
***

### 预览 GitHub 项目里的网页或 Demo
>在项目源代码页面链接前缀那加上http://htmlpreview.github.com/?
http://htmlpreview.github.com/

<hr/>

### git 不提交并且切换分支 
>在 A 分支 git add -A 暂存
>在 A 分支 git stash 隐藏当前暂存
>切换分支 正常操作
>切换回A分支 git  stash apply 找回隐藏暂存

<hr />

### git + svn

#### 折衷方案[假设全都不了解svn,git]

git不能忽略svn所有配置文件，隐藏文件
svn不能忽略git所有配置文件，隐藏文件

<hr/>

>提交流程

>>svn add . --force

>>svn update

>>svn commit -m ""

>>git add -A

>>git commit -m ""

>>git push --force origin 2-feture-xxx

### gitlab 时间跟踪
>https://gitlab.com/help/workflow/time_tracking.md

>只能在评论处输入以更新时间

>估计
要输入估算值，请写入/estimate，然后输入时间。例如，如果您需要输入3天，5小时和10分钟的估计值，您可以写信
 /estimate 3d 5h 10m。我们支持的时间单位列在本帮助页面的底部。
每次输入新的时间估算值时，此新值都会覆盖之前的任何时间估算值。在问题或合并请求中应该只有一个有效的估计值。
要完全删除估算，请使用/remove_estimate。

>所花费的时间
要输入花费的时间，请使用/spend 3d 5h 10m。
每次进入的新时间都将添加到当前为问题或合并请求花费的总时间。
您可以通过输入负数来删除时间：/spend -3d将从花费的总时间中删除3天。你不能花费0分钟以上的时间，因此如果你删除了比已经输入的时间更长的时间，GitLab将自动重置所花费的时间。
要删除一次花费的所有时间，请使用/remove_time_spent。

>组态
以下时间单位可用：
周（w）
天（d）
小时（h）
分钟（m）

>默认转化率是
1周= 5天，
1天= 8小时。

>其他有趣的链接：
about.gitlab.com上的时间跟踪登陆页面


### 阿里云git

>https://code.aliyun.com

>使用阿里系账号登陆系统


>推送仓库注意：

>推送账号密码与登陆账号密码互相独立，首次推送代码之前需要重置阿里git密码


>code.aliyun.com的用户名和密码设置的url分别为

>https://code.aliyun.com/profile

>https://code.aliyun.com/profile/password/edit

>进入到 https://code.aliyun.com/profile/password/edit， 点击 忘记密码，系统会发送一个重置密码链接，回到邮件打开输入新的密码就可以了，像楼上说的，如果没收到就到垃圾邮箱查看下

>https://yq.aliyun.com/ask/52487

### 在Mac、Linux 终端显示 Git 当前所在分支
>1. 进入你的home目录

```bash
cd ~
```
> 编辑.bashrc文件

```bash
vi .bashrc
```
>将下面的代码加入到文件的最后处

```bash
function git_branch {
   branch="`git branch 2>/dev/null | grep "^\*" | sed -e "s/^\*\ //"`"
   if [ "${branch}" != "" ];then
       if [ "${branch}" = "(no branch)" ];then
           branch="(`git rev-parse --short HEAD`...)"
       fi
       echo " ($branch)"
   fi
}
export PS1='\u@\h \[\033[01;36m\]\W\[\033[01;32m\]$(git_branch)\[\033[00m\] \$ '
```
>保存退出

```bash
// 点击ESC键
// 输入 
:wq
```

>2. 执行加载命令

```bash
source ./.bashrc
```
>完成

>Mac 下面启动的 shell 是 login shell，所以加载的配置文件是.bash_profile，不会加载.bashrc。如果你是 Mac 用户的话，需要再执行下面的命令，这样每次开机后才会自动生效：

```bash
echo "[ -r ~/.bashrc ] && source ~/.bashrc" >> .bash_profile
```

### 如何创建公钥

>首先启动一个Git Bash窗口（非Windows用户直接打开终端）

>执行：

```bash
cd ~/.ssh
```
>如果返回“… No such file or directory”，说明没有生成过SSH Key，直接进入第4步。否则进入第3步备份!

>备份：

```bash
mkdir key_backup mv id_isa* key_backup
```
>生成新的Key：（引号内的内容替换为你自己的邮箱）

```bash
ssh-keygen -t rsa -C "your_email@youremail.com"
```
>输出显示：

```bash
>Generating public/private rsa key pair. Enter file in which to save the key 
(/Users/your_user_directory/.ssh/id_rsa):<press enter>
```
>直接回车，不要修改默认路劲。

```bash
>Enter passphrase (empty for no passphrase):<enter a passphrase>
Enter same passphrase again:<enter passphrase again>
```
>设置一个密码短语，在每次远程操作之前会要求输入密码短语！闲麻烦可以直接回车，不设置。

>成功：

```bash
Your identification has been saved in /Users/your_user_directory/.ssh/id_rsa. Your public key has been saved in /Users/your_user_directory/.ssh/id_rsa.pub. The key fingerprint is: ... ...
```
>提交公钥：

>6.1 找到.ssh文件夹，用文本编辑器打开“id_rsa.pub”文件，复制内容到剪贴板。

>6.2 打开 https://github.com/settings/ssh ，点击 Add SSH Key 按钮，粘贴进去保存即可。


### 让终端支持socks5代理协议和git加速

>只针对当前终端窗口有效

```bash
export http_proxy=socks5://127.0.0.1:1080
export https_proxy=socks5://127.0.0.1:1080
```

>git代理  永久

```bash
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

>取消git代理

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

***

>修改缓冲区大小

>RPC failed; curl 18 transfer closed with outstanding read data remaining

```bash
Git config --global http.postBuffer 524288000
// http.postBuffer 后，单位是b，524288000B也就500M左右
```

### 忽略已在版本控制中的文件

```bash
git rm --cached logs/xx.log -r
```
>然后更新 .gitignore 忽略掉目标文件
>最后 

```bash
git commit -m "We really don't want Git to track this anymore!"
```
***

### gitlab 添加 倍洽 webhook ，配置 gitlab 对倍洽自动发送项目消息通知
>倍洽app

>联系人

>设置

>机器人

>创建机器人

>进入配置网页

>添加机器人

>选择 gitlab

>添加

>选择发送目标

>复制webhook URL

>粘贴到 gitlab 指定项目的 -设置-集成-url 

>完成

### 冲突解决
#### 在当前分支 1-feature-test 下执行
>git add -A

>git cimmit -m "feat: some,#1"

>git checkout develop

>git pull

>git checkout 1-feature-test

>git rebase develop

>解决冲突[如果有冲突]

>git add -A

>git rebase --continue

>冲突解决

>git push origin xxxx --force


