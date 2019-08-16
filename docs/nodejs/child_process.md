#### child_process

##### spawn

```ts
const puppeteer = require('puppeteer');
const path = require('path');
const fs = require('fs');
const {spawn} = require('child_process');

// 开启子进程，执行 ls -l 命令以查询当前文件夹下的文件列表
const process = spawn('ls', ['-l'])

const {stdin, stdout} = process;

// 监控上一个子进程的执行输出，获取输出值，并打印
stdout.on('data', (data) => {
    console.log(`stdout: ${data}`);
});
```

***

##### exec
```ts
const puppeteer = require('puppeteer');
const path = require('path');
const fs = require('fs');
const process = require('child_process');
const lscmd = process.exec('ls -l', function (e, stdout, stderr) {
    if (!e) {
        console.log('stdout:', stdout)
        console.log('stderr:', stderr)
        return
    }
    console.log('e:', e)
})

lscmd.on('exit', status => {
    if (status) {
        throw new Error(`process ended with status: ${status}`);
    }
    console.log('process finish')
});
```