#### sentry 使用方法

#### 1:获取密钥与项目号

>进入项目-设置
![1561543286706](https://user-images.githubusercontent.com/30850497/60171108-877bad80-983c-11e9-9ea8-a09a38fb9d17.jpg)

>左侧列表-客户端密钥 DSN
![1561543474300](https://user-images.githubusercontent.com/30850497/60171282-dfb2af80-983c-11e9-89a2-705528934486.jpg)


>点击下方-展开

>sentry_key 的值即为密钥

>每个项目可创建多个独立密钥以管理

![1561543618943](https://user-images.githubusercontent.com/30850497/60171444-36b88480-983d-11e9-8772-f1cbf77772d1.jpg)

>获取项目号

>api后的数字即为项目号

![1561543823528](https://user-images.githubusercontent.com/30850497/60171701-af1f4580-983d-11e9-8bd1-69d9ac8c93cd.jpg)

***

#### 2:组合密钥链接
>key 密钥

>sentry.io 服务器地址

>project 项目号

```ts
Sentry.init({ dsn: 'https://<key>@sentry.io/<project>' });
```

***

#### 3:在项目中引入 sentry sdk

>javascript 版本

```bash
yarn add @sentry/browser@5.4.3

npm install @sentry/browser@5.4.3 -S
```
```ts
import * as Sentry from '@sentry/browser';
```



>python 版本

```bash
pip install --upgrade sentry-sdk==0.9.2
```
```
import sentry_sdk

//Sentry.init({ dsn: 'https://1ef0xxxxxxxxxe513@sentry.io/123' });
Sentry.init({ dsn: 'https://c53xxxxxxxxxxxxxxx94a24@xx.xxxx.com/99999999' });
```
***

#### 4:在项目初始化全局 sentry 监听

>应用程序加载期间，您应该尽快使用Sentry SDK

>应在在项目启动初期声明监听事件

```bash
//Sentry.init({ dsn: 'https://1ef0xxxxxxxxxe513@sentry.io/123' });
Sentry.init({ dsn: 'https://c53xxxxxxxxxxxxxxx94a24@xx.xxxx.com/99999999' });
```

***

#### 5:倍洽关联 sentry 监听，推送日志消息到倍洽
>获取倍洽webhook链接 xxxlink

>sentry-指定项目-设置-Legacy Integrations-开启WebHooks

>刷新

>sentry-指定项目-设置-Legacy Integrations-WebHooks-添加链接xxxlink