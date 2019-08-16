#### pm2

>https://pm2.io/doc/zh/runtime/overview/


>nodejs 进程管理工具

>npm I pm2 -g

>pm2配置

>pm2.yml 

```yml
apps:
    // 使用pm2运行的文件
  - script: ./server/server.js

   // 为启动的服务指定名称
    name: vue-todo

    // 启动服务时的可用环境变量
    env_production:
      NODE_ENV: production

      // 只能本地访问，禁止外网访问
      HOST: localhost

      PORT: 8888
    
    env_development:
          NODE_ENV: development
    
          // 只能本地访问，禁止外网访问
          HOST: localhost
    
          PORT: 3333
```

##### 使用pm2启动服务端server.js

>--env production ,表示使用pm2.yml  内 env_production的环境变量

```sh
pm2 start pm2.yml --env production
```

##### 使用pm2重启服务端server.js

```sh
pm2 restart vue-todo
```

##### 使用pm2停止服务端server.js

```sh
pm2 stop vue-todo
```

##### 使用pm2查看已启动的服务

```sh
pm2 list
```

##### 使用pm2查看指定服务的日志

```sh
pm2 log vue-todo
```

#### 服务端部署

```bash
// 连接到服务器
ssh root@tom

// 克隆仓库
git clone git@xxx.com:test/vue-todo.git

// 安装依赖
cd vue-todo
npm i

// 构建正式环境文件
npm run build

// 启动项目服务端渲染服务器
pm2 start pm2.yml --env production
```

#### 使用nginx 反向代理nodejs服务

>nginx 监听80端口，将对80端口的请求转发到nodejs监听的本地其他端口

##### 安装nginx

```bash
yum install nginx
```

##### linux 配置nginx

```bash
cd /etc.nginx
cd conf.d/
vim todo.conf

service nginx reload
```