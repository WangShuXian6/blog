#### Flutter
>https://flutter.io/

>https://github.com/flutter/flutter

>环境变量

```bash
export PATH=/Users/zl/code/flutter/bin:$PATH

export PUB_HOSTED_URL=https://pub.flutter-io.cn

export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

```bash
vim ~/.bash_profile
source ~/.bash_profile
```

>新建必要文件夹并授权

> /usr/local/ 新建 Cellar,opt,Frameworks 文件夹

>授权

```bash
sudo chown -R $(whoami) /usr/local/Cellar
sudo chown -R `whoami`:admin /usr/local/opt
sudo chown -R `whoami`:admin /usr/local/Frameworks
sudo chown -R `whoami`:admin /usr/local/bin
sudo chown -R `whoami`:admin /usr/local/share
```

```bash
xcode-select --install
```

>Download brew at https://brew.sh/

>检查环境

>flutter doctor

>同步项目的包

>flutter packages get