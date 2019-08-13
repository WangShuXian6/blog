#### Angular 跨域
>https://github.com/angular/angular-cli/blob/master/docs/documentation/stories/proxy.md

>假如待请求的接口服务器为 https://xxx.xxxxxxx.com/api/card/login

>项目根目录

>proxy.config.json

>target 为接口服务器

```JSON
{
  "/api": {
    "target": "https://xxx.xxx.com/api",
    "changeOrigin": true,
    "secure": false,
    "logLevel": "debug",
    "pathRewrite": {
      "^/api": ""
    }
  }
}
```

```ts
export class AuthenticationService implements AuthService {
    API_URL = '/api/';
    API_ENDPOINT_LOGIN = 'card/Login';

    public login(credential: Credential): Observable<any> {
        // Expecting response from API
        // tslint:disable-next-line:max-line-length
        // {"id":1,"username":"admin","password":"demo","email":"admin@demo.com","accessToken":"access-token-0.022563452858263444","refreshToken":"access-token-0.9348573301432961","roles":["ADMIN"],"pic":"./assets/app/media/img/users/user4.jpg","fullname":"Mark Andre"}
        return this.http.get<AccessData>(this.API_URL + this.API_ENDPOINT_LOGIN + '?' + this.util.urlParam(credential)).pipe(
            map((result: any) => {
                if (result instanceof Array) {
                    return result.pop();
                }
                return result;
            }),
            tap(this.saveAccessData.bind(this)),
            catchError(this.handleError('login', []))
        );
    }
}
```

***

#### Proxy To Backend
>Using the proxying support in webpack's dev server we can highjack certain URLs and send them to a backend server. We do this by passing a file to --proxy-config

>Say we have a server running on http://localhost:3000/api and we want all calls to >http://localhost:4200/api to go to that server.

>We create a file next to our project's package.json called proxy.conf.json with the content

```ts
{
  "/api": {
    "target": "http://localhost:3000",
    "secure": false
  }
}
```
>You can read more about what options are available here.

>We can then add the proxyConfig option to the serve target:

```JSON
"architect": {
  "serve": {
    "builder": "@angular-devkit/build-angular:dev-server",
    "options": {
      "browserTarget": "your-application-name:build",
      "proxyConfig": "proxy.conf.json"
    },
```
>Now in order to run our dev server with our proxy config we can call ng serve.

>After each edit to the proxy.conf.json file remember to relaunch the ng serve process to make your changes effective.

>Rewriting the URL path

>One option that comes up a lot is rewriting the URL path for the proxy. This is supported by the pathRewrite option.

>Say we have a server running on http://localhost:3000 and we want all calls to >http://localhost:4200/api to go to that server.

>In our proxy.conf.json file, we add the following content

```JSON
{
  "/api": {
    "target": "http://localhost:3000",
    "secure": false,
    "pathRewrite": {
      "^/api": ""
    }
  }
}
```
>If you need to access a backend that is not on localhost, you will need to add the changeOrigin option as follows:

```JSON
{
  "/api": {
    "target": "http://npmjs.org",
    "secure": false,
    "pathRewrite": {
      "^/api": ""
    },
    "changeOrigin": true
  }
}
```
>To help debug whether or not your proxy is working properly, you can also add the logLevel option as follows:

```JSON
{
  "/api": {
    "target": "http://localhost:3000",
    "secure": false,
    "pathRewrite": {
      "^/api": ""
    },
    "logLevel": "debug"
  }
}
```
>Possible options for logLevel include debug, info, warn, error, and silent (default is info)

>Multiple entries

>If you need to proxy multiple entries to the same target define the configuration in proxy.conf.js instead of proxy.conf.json e.g.

```ts
const PROXY_CONFIG = [
    {
        context: [
            "/my",
            "/many",
            "/endpoints",
            "/i",
            "/need",
            "/to",
            "/proxy"
        ],
        target: "http://localhost:3000",
        secure: false
    }
]

module.exports = PROXY_CONFIG;
```
>Make sure to point to the right file (.js instead of .json):

```ts
"architect": {
  "serve": {
    "builder": "@angular-devkit/build-angular:dev-server",
    "options": {
      "browserTarget": "your-application-name:build",
      "proxyConfig": "proxy.conf.js"
    },
```
>Bypass the Proxy

>If you need to optionally bypass the proxy, or dynamically change the request before it's sent, define the configuration in proxy.conf.js e.g.

```ts
const PROXY_CONFIG = {
    "/api/proxy": {
        "target": "http://localhost:3000",
        "secure": false,
        "bypass": function (req, res, proxyOptions) {
            if (req.headers.accept.indexOf("html") !== -1) {
                console.log("Skipping proxy for browser request.");
                return "/index.html";
            }
            req.headers["X-Custom-Header"] = "yes";
        }
    }
}

module.exports = PROXY_CONFIG;
```
>Using corporate proxy

>If you work behind a corporate proxy, the regular configuration will not work if you try to proxy calls to any URL outside your local network.

>In this case, you can configure the backend proxy to redirect calls through your corporate proxy using an agent:

>npm install --save-dev https-proxy-agent

>Then instead of using a proxy.conf.json file, we create a file called proxy.conf.js with the following content:

```ts
var HttpsProxyAgent = require('https-proxy-agent');
var proxyConfig = [{
  context: '/api',
  target: 'http://your-remote-server.com:3000',
  secure: false
}];

function setupForCorporateProxy(proxyConfig) {
  var proxyServer = process.env.http_proxy || process.env.HTTP_PROXY;
  if (proxyServer) {
    var agent = new HttpsProxyAgent(proxyServer);
    console.log('Using corporate proxy server: ' + proxyServer);
    proxyConfig.forEach(function(entry) {
      entry.agent = agent;
    });
  }
  return proxyConfig;
}

module.exports = setupForCorporateProxy(proxyConfig);
```
>This way if you have a http_proxy or HTTP_PROXY environment variable defined, an agent will automatically be added to pass calls through your corporate proxy when running npm start.