#### animation

#### 放大动效

```less
.dynamic-blown-up {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 200px;
  height: 50px;
  border-radius: 8px;
  background-color: #64e7fd;
  animation: blownUp 0.3s linear 0s 1;
  font-size: 10px;
  color: #ff6209;
}

@keyframes blownUp {
  0% {
    transform: scale(0, 0);
  }

  90% {
    transform: scale(1.2, 1.2);
  }

  100% {
    transform: scale(1, 1);
  }
}
```

<hr/>

#### 消失动效

```less
.dynamic-disappear {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 200px;
  height: 50px;
  border-radius: 8px;
  background-color: #64e7fd;
  animation: disappear 0.3s linear 0s 1 forwards;
  font-size: 10px;
  color: #ff6209;
}

@keyframes disappear {
  0% {
    transform: scale(1.2, 1.2);

  }

  30% {
    transform: scale(1, 1);
  }

  100% {
    transform: scale(0, 0);
  }
}
```

<hr/>

#### 弹跳动效
```less
.dynamic-bounce {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 200px;
  height: 50px;
  border-radius: 8px;
  background-color: #64e7fd;
  animation: bounce 0.3s linear 0s 1 forwards;
  font-size: 10px;
  color: #ff6209;
}

@keyframes bounce {
  0% {
    transform: scale(0, 0);
  }

  30% {
    transform: scale(1.2, 1.2);
  }

  60% {
    transform: scale(0.7, 0.7);
  }

  100% {
    transform: scale(1, 1);
  }
}
```

##### 旋转动画
```less
.bgm-button {
            position: fixed;
            top: 50px;
            right: 30px;
            display: flex;
            width: 30px;
            height: 30px;
        }

        .bgm-button.play {
            animation: bgmButtonRotate 2s infinite linear;
        }

        @keyframes bgmButtonRotate {
            from {
                transform: rotateZ(0deg);
            }
            to {
                transform: rotateZ(360deg);
            }
        }
```

***
#### loading

![image](https://user-images.githubusercontent.com/30850497/60857508-76b83800-a23d-11e9-8d36-2dc117deee5b.png)
>预览 https://codepen.io/WangShuXian6/pen/KjrBrd

```html
<!DOCTYPE html>
<html lang="zh-cn">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <base href="" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no, minimum-scale=1.0">

    <style>
        .midoci-loading-wrapper {
            z-index: 100;
            position: fixed;
            left: 0;
            top: 0;
            width: 100vw;
            height: 100vh;

            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background-color: #1c1c1d;
        }

        .midoci-loading {
            display: flex;
            align-items: flex-end;
            justify-content: space-between;

            width: 5rem;
            height: 4rem;
            overflow: hidden;
        }

        .midoci-loading .midoci-line {
            display: flex;
            width: 0.2rem;
            height: 1rem;
            background-color: #be9467;
            border-radius: 1px;
            animation: midociLoading 0.7s ease-in-out infinite;
        }

        .midoci-loading-wrapper .midoci-tip {
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem;
            color: #be9467;
        }

        .midoci-line:nth-child(1) {
            animation-delay: 0s;
        }

        .midoci-line:nth-child(2) {
            animation-delay: 0.1s;
        }

        .midoci-line:nth-child(3) {
            animation-delay: 0.2s;
        }

        .midoci-line:nth-child(4) {
            animation-delay: 0.3s;
        }

        .midoci-line:nth-child(5) {
            animation-delay: 0.4s;
        }

        .midoci-line:nth-child(6) {
            animation-delay: 0.5s;
        }

        .midoci-line:nth-child(7) {
            animation-delay: 0.6s;
        }

        @keyframes midociLoading {
            0% {
                transform: scaleY(1)
            }

            50% {
                transform: scaleY(6)
            }

            100% {
                transform: scaleY(1)
            }
        }
    </style>
    <title>loading/title>
</head>

<body>
    <div class="midoci-loading-wrapper">
        <div class="midoci-loading">
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
        </div>
        <span class="midoci-tip">LOADING...</span>
    </div>
</body>

</html>
```

#### loading use rem
>1rem=100px when width=375px

```html
<!DOCTYPE html>
<html lang="zh-cn">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <base href="" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no, minimum-scale=1.0">

    <style>
        html {
            font-size: 26.666667vw;
            box-sizing: border-box;
            background-color: #000;
        }

        .midoci-loading-wrapper {
            z-index: 100;
            position: fixed;
            left: 0;
            top: 0;
            width: 100vw;
            height: 100vh;

            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background-color: #1c1c1d;
        }

        .midoci-loading {
            display: flex;
            align-items: flex-end;
            justify-content: space-between;

            width: 0.6665rem;
            height: 0.45rem;
            overflow: hidden;
        }

        .midoci-loading .midoci-line {
            display: flex;
            width: 0.02rem;
            height: 0.1333335rem;
            background-color: #be9467;
            border-radius: 1px;
            animation: midociLoading 0.7s ease-in-out infinite;
        }

        .midoci-loading-wrapper .midoci-tip {
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.133333rem;
            color: #be9467;
        }

        .midoci-line:nth-child(1) {
            animation-delay: 0s;
        }

        .midoci-line:nth-child(2) {
            animation-delay: 0.1s;
        }

        .midoci-line:nth-child(3) {
            animation-delay: 0.2s;
        }

        .midoci-line:nth-child(4) {
            animation-delay: 0.3s;
        }

        .midoci-line:nth-child(5) {
            animation-delay: 0.4s;
        }

        .midoci-line:nth-child(6) {
            animation-delay: 0.5s;
        }

        .midoci-line:nth-child(7) {
            animation-delay: 0.6s;
        }

        @keyframes midociLoading {
            0% {
                transform: scaleY(1)
            }

            50% {
                transform: scaleY(6)
            }

            100% {
                transform: scaleY(1)
            }
        }
    </style>
    <title>loading</title>
</head>

<body>
    <div class="midoci-loading-wrapper">
        <div class="midoci-loading">
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
            <div class="midoci-line"></div>
        </div>
        <span class="midoci-tip">LOADING...</span>
    </div>
</body>

</html>
```