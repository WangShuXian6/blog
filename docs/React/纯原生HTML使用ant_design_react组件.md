#### 纯原生 HTML 使用 ant design react 组件

>依赖库打包 

[umd.zip](https://github.com/WangShuXian6/blog/files/3425474/umd.zip)


>antd.DatePicker

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <title>ant design in html</title>
  <link rel="stylesheet" href="./umd/antd.css">
  <script src="./umd/react.production.min.js"></script>
  <script src="./umd/react-dom.production.min.js"></script>
  <script src="./umd/browser5.8.24.js"></script>
  <script src="./umd/moment-with-locales.js"></script>
  <script src="./umd/antd-with-locales.js"></script>
  <script></script>
</head>
<body>

<div id="date"></div>

<script>
  moment.locale('zh-cn');

  const dateDom = document.querySelector('#date')

  const reactEle = React.createElement(antd.DatePicker, {
    onChange: (e) => {
      console.log(e)
      console.log(e._d)
      console.log(new Date(e._d).toLocaleTimeString())
    }
  })

  ReactDOM.render(reactEle, dateDom)
</script>
</body>
</html>

```
***
>antd.Table

```HTML
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <title>ant design in html</title>
  <link rel="stylesheet" href="./umd/antd.css">
  <script src="./umd/react.production.min.js"></script>
  <script src="./umd/react-dom.production.min.js"></script>
  <script src="./umd/browser5.8.24.js"></script>
  <script src="./umd/moment-with-locales.js"></script>
  <script src="./umd/antd-with-locales.js"></script>
  <script></script>
</head>
<body>

<div id="date"></div>

<script>
  moment.locale('zh-cn');

  const dataSource = [
    {
      key: '1',
      name: '胡彦斌',
      age: 32,
      address: '西湖区湖底公园1号',
    },
    {
      key: '2',
      name: '胡彦祖',
      age: 42,
      address: '西湖区湖底公园1号',
    },
  ];

  const columns = [
    {
      title: '姓名',
      dataIndex: 'name',
      key: 'name',
    },
    {
      title: '年龄',
      dataIndex: 'age',
      key: 'age',
    },
    {
      title: '住址',
      dataIndex: 'address',
      key: 'address',
    },
  ];

  const dateDom = document.querySelector('#date')

  const reactEle = React.createElement(antd.Table, {
    dataSource,
    columns,
    onChange: (e) => {
      console.log(e)
      console.log(e._d)
      console.log(new Date(e._d).toLocaleTimeString())
    }
  })

  ReactDOM.render(reactEle, dateDom)
</script>
</body>
</html>

```
***
>demo

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <title>ant design in html</title>
  <link rel="stylesheet" href="./umd/antd.css">
  <script src="./umd/react.production.min.js"></script>
  <script src="./umd/react-dom.production.min.js"></script>
  <script src="./umd/browser5.8.24.js"></script>
  <script src="./umd/moment-with-locales.js"></script>
  <script src="./umd/antd-with-locales.js"></script>
  <script></script>
</head>
<body>

<div id="date"></div>


<script>
  const dateDom = document.querySelector('#date')

  class LikeButton extends React.Component {
    constructor(props) {
      super(props);
      this.state = {liked: false};
    }

    render() {
      if (this.state.liked) {
        return 'You liked this.';
      }

      return React.createElement(
        'button',
        {
          onClick: () => {
            console.log(123)
            this.setState({liked: true})
          }
        },
        'Like'
      );
    }
  }

  ReactDOM.render(React.createElement(LikeButton), dateDom);
</script>
</body>
</html>

```