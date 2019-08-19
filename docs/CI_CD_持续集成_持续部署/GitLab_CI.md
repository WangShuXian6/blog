#### GitLab CI 
>https://docs.gitlab.com.cn  GitLab 中文文档

>GitLab CI 是 GitLab 为了提升其在软件开发工程中作用，完善 DevOPS 理念所加入的 CI/CD 基础功能。可以便捷的融入软件开发环节中。通过 GitLab CI 可以定义完善的 CI/CD Pipeline。

##### 优势
> - GitLab CI 是默认包含在 GitLab 中的，我们的代码使用 GitLab 进行托管，这样可以很容易的进行集成
> - GitLab CI 的前端界面比较美观，容易被人接受
> - 包含实时构建日志，容易追踪
> - 采用 C/S 的架构，可方面的进行横向扩展，性能上不会有影响
> - 使用 YAML 进行配置，任何人都可以很方便的使用。

***

##### Pipeline
>Pipeline 相当于一个构建任务，里面可以包含多个流程，如依赖安装、编译、测试、部署等。 任何提交或者 Merge Request 的合并都可以触发 Pipeline

##### Stages
>Stage 表示构建的阶段，即上面提到的流程.

> - 所有 Stages 按顺序执行，即当一个 Stage 完成后，下一个 Stage 才会开始
> - 任一 Stage 失败，后面的 Stages 将永不会执行，Pipeline 失败
> - 只有当所有 Stages 完成后，Pipeline 才会成功

##### Jobs
>Job 是 Stage 中的任务.

> - 相同 Stage 中的 Jobs 会并行执行
> - 任一 Job 失败，那么 Stage 失败，Pipeline 失败
> - 相同 Stage 中的 Jobs 都执行成功时，该 Stage 成功

***

##### GitLab runner 实际执行者
>Runner 是任务的实际执行者， 可以在 MacOS/Linux/Windows 等系统上运行。使用 golang 进行开发。 同时也可部署在 k8s 上

>将 runner 注册为一个容器

```bash
docker run --rm -t -i -v /path/to/config:/etc/gitlab-runner --name gitlab-runner gitlab/gitlab-runner register \
  --executor "docker" \
  --docker-image alpine:3 \
  --url "https://gitlab.com/" \
  --registration-token "PROJECT_REGISTRATION_TOKEN" \
  --description "docker-runner" \
  --tag-list "dev" \
  --run-untagged \
  --locked="true"
```

>在物理机上的注册方式

```bash
sudo gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.com/" \
  --registration-token "PROJECT_REGISTRATION_TOKEN" \
  --executor "docker" \
  --docker-image alpine:3 \
  --description "docker-runner" \
  --tag-list "docker,aws" \
  --run-untagged \
  --locked="false" \
```

>runner 的类型

> - Shared - Runner runs jobs from all unassigned projects
> - Group - Runner runs jobs from all unassigned projects in its group
> - Specific - Runner runs jobs from assigned projects
> - Locked - Runner cannot be assigned to other projects
> - Paused - Runner will not receive any new jobs

***
##### .gitlab-ci.yml 执行 CI
>