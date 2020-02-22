#### 自动化异常提醒

#### 独立安装

>https://docs.sentry.io/server/installation/docker/

***
#### 合并安装

>https://github.com/getsentry/onpremise

##### 安装 docker-compose

```bash
sudo pip install -U docker-compose

docker-compose --version
```

##### docker 安装 sentry

```bash
git clone https://github.com/getsentry/onpremise.git
```

>进入 clone 下来的 onpremise 目录依次执行

```bash
cd onpremise

docker volume create --name=sentry-data && docker volume create --name=sentry-postgres

cp -n .env.example .env
```

>获取项目的 key
```bash
docker-compose build

docker-compose run --rm web config generate-secret-key
```

>复制获取到的 key 字符串,作为 .env  SENTRY_SECRET_KEY 的值
```bash
vi .env
```

>初始化数据库，该过程会让你输入 用户邮箱 和密码 一路走下去 即可
```bash
docker-compose run --rm web upgrade
```

>启动服务
```bash
docker-compose up -d
```

>localhost:9000
***

>更新
```bash
docker-compose build --pull # Build the services again after updating, and make sure we're up to date on patch version
docker-compose run --rm web upgrade # Run new migrations
docker-compose up -d # Recreate the services
```