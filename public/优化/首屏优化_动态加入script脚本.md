###$ 首屏优化，动态加入 script 脚本

>优化前

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <title>norm</title>

  <style>
    .main {
      display: block;
      width: 10vw;
      height: 10vh;
      background-color: beige;
    }

    .fixed {
      position: fixed;
      right: 10px;
      top: 10px;
      color: red;
    }
  </style>

  <script>
    function measureCRP() {
      let t = window.performance.timing,
        interactive = t.domInteractive - t.domLoading,
        dcl = t.domContentLoadedEventStart - t.domLoading,
        complete = t.domComplete - t.domLoading;
      let stats = document.createElement('p');

      stats.innerHTML = `
        <h5>[白屏]--DOM 准备就绪 interactive: ${interactive} ms</h5>
        <h5>[可交互]--DOM 和 CSSOM 均准备就绪 dcl: ${dcl} ms</h5>
        <h5>[完全加载]--网页及其所有子资源都准备就绪 ms, complete: ${complete} ms</h5>
      `
      stats.classList = 'fixed'
      // stats.textContent = ' DOM 准备就绪 interactive: ' + interactive + 'ms, ' +
      //   'DOM 和 CSSOM 均准备就绪 dcl: ' + dcl + ' 网页及其所有子资源都准备就绪 ms, complete: ' + complete + 'ms';
      document.body.appendChild(stats);
    }
  </script>
</head>
<body>

<script src="js/a_1.js"></script>
<script src="js/a_2.js"></script>
<script src="js/a_3.js"></script>
<script src="js/a_4.js"></script>
<script src="js/a_5.js"></script>
<script src="js/a_6.js"></script>

<script>
  window.onload = function () {
    measureCRP()
  }
</script>

<div class="main">

</div>

</body>
</html>
```

>优化后

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <title>auto</title>

  <style>
    .main {
      display: block;
      width: 10vw;
      height: 10vh;
      background-color: beige;
    }

    .fixed {
      position: fixed;
      right: 10px;
      top: 10px;
      color: red;
    }
  </style>

  <script>
    function measureCRP() {
      let t = window.performance.timing,
        interactive = t.domInteractive - t.domLoading,
        dcl = t.domContentLoadedEventStart - t.domLoading,
        complete = t.domComplete - t.domLoading;
      let stats = document.createElement('p');

      stats.innerHTML = `
        <h5>[白屏]--DOM 准备就绪 interactive: ${interactive} ms</h5>
        <h5>[可交互]--DOM 和 CSSOM 均准备就绪 dcl: ${dcl} ms</h5>
        <h5>[完全加载]--网页及其所有子资源都准备就绪 ms, complete: ${complete} ms</h5>
      `
      stats.classList = 'fixed'
      // stats.textContent = ' DOM 准备就绪 interactive: ' + interactive + 'ms, ' +
      //   'DOM 和 CSSOM 均准备就绪 dcl: ' + dcl + ' 网页及其所有子资源都准备就绪 ms, complete: ' + complete + 'ms';
      document.body.appendChild(stats);
    }
  </script>
</head>
<body>

<script>

  const scriptUrls = ['js/a_1.js', 'js/a_2.js', 'js/a_3.js', 'js/a_4.js', 'js/a_5.js', 'js/a_6.js']

  function autoScriptcallback(totalNumber, finishedNumber) {
    console.log('已加载成功一条 script', totalNumber, '----', finishedNumber)
  }

  function autoScript(scriptUrls) {
    const totalNumber = scriptUrls.length
    let finishedNumber = 0
    scriptUrls.forEach((scriptUrl) => {
      // 插入script标签并监听加载
      let head = document.getElementsByTagName('head')[0];
      let script = document.createElement('script');
      script.type = 'text/javascript';
      script.onload = script.onreadystatechange = function () {
        if (!this.readyState || this.readyState === "loaded"
          || this.readyState === "complete") {
          finishedNumber++
          autoScriptcallback(totalNumber, finishedNumber);
          // Handle memory leak in IE
          script.onload = script.onreadystatechange = null;
        }
      };
      script.src = scriptUrl;
      head.appendChild(script);
    })
  }

  window.onload = function () {
    measureCRP()
    autoScript(scriptUrls)
  }
</script>


<div class="main">

</div>

</body>
</html>
```