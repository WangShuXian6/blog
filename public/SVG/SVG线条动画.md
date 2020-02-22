#### SVG 线条动画

> IVWEB 线条动画

>https://codepen.io/WangShuXian6/pen/KEBQYw

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <style>
    .text {
      fill: none;
      stroke-width: 5;
      stroke-dasharray: 0 300;
      stroke-dashoffset: 0;
    }

    .text:nth-child(3n + 1) {
      stroke: #fa637f;
      animation: stroke 6s ease-in-out forwards;

    }

    .text:nth-child(3n + 2) {
      stroke: #92d0fa;
      animation: stroke1 6s ease-in-out forwards;

    }

    .text:nth-child(3n + 3) {
      stroke: #fbbe37;
      animation: stroke2 6s ease-in-out forwards;

    }


    @keyframes stroke {
      100% {
        stroke-dashoffset: 1000;
        stroke-dasharray: 80 160;
      }
    }

    @keyframes stroke1 {
      100% {
        stroke-dashoffset: 1080;
        stroke-dasharray: 80 160;
      }
    }


    @keyframes stroke2 {
      100% {
        stroke-dashoffset: 1160;
        stroke-dasharray: 80 160;
      }
    }

    /* Other styles */
    html, body {
      height: 100%;
    }

    body {
      background: #212121;
      background-size: .2em 100%;
      font: 14.5em/1 Open Sans, Impact;
      text-transform: uppercase;
      margin: 0;
    }

    svg {
      position: absolute;
      width: 100%;
      height: 100%;
    }
  </style>
  <title>Title</title>
</head>
<body>

<svg viewBox="0 0 1320 300">

  <!-- Symbol -->
  <symbol id="s-text">
    <text text-anchor="middle"
          x="50%" y="50%" dy=".35em">
      MIDOCI
    </text>
  </symbol>

  <!-- Duplicate symbols -->
  <use xlink:href="#s-text" class="text"
  ></use>
  <use xlink:href="#s-text" class="text"
  ></use>
  <use xlink:href="#s-text" class="text"
  ></use>


</svg>

</body>
</html>

```

![Kapture 2019-03-19 at 15 40 38](https://user-images.githubusercontent.com/30850497/54588253-647f7780-4a5d-11e9-9221-6e8975d24601.gif)

***

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>SVG</title>
  <style type="text/css">

    svg {
      position: absolute;
      width: 100%;
      height: 100%;
      background-color: #fff;
    }

    path {
      fill: transparent;
      stroke: #92d0fa;
      stroke-dasharray: 255%;
      animation: outline 5s infinite alternate;
    }

    @keyframes outline {
      0% {
        stroke-dashoffset: 255%;
      }
      100% {
        stroke-dashoffset: 0;
      }
    }
  </style>
</head>
<body>

<svg version="1.1"
     xmlns="http://www.w3.org/2000/svg" viewBox="0 0 60 75">
  <symbol id="pathSymbol">
    <path class="path" stroke="#00adef"
          d="M31.52,4.18l.15,0h0a7.42,7.42,0,0,1,8.2,8.26h0a1.56,1.56,0,0,0,0,.16,1.59,1.59,0,0,0,3.17.19h0A10.6,10.6,0,0,0,32.51.94,10.71,10.71,0,0,0,31.32,1h0a1.59,1.59,0,0,0,.19,3.17Z"
    />
    <path class="path" stroke="#00adef"
          d="M34.41,12.51h0a1.59,1.59,0,0,0,3.19,0s0,0,0,0h0c0-.05,0-.1,0-.15s0-.1,0-.14a6.36,6.36,0,0,0-5.7-5.57,1.59,1.59,0,1,0-.39,3.15h0A3.19,3.19,0,0,1,34.41,12.51Z"
    />
    <path class="path" stroke="#00adef"
          d="M54.71,32.71a15.41,15.41,0,0,0-20.5-12A10.25,10.25,0,0,0,24,10.83a10.53,10.53,0,0,0-2.79.4,21.76,21.76,0,0,0-15.26,15h0A119.15,119.15,0,0,0,2.8,39.59c-1.61,9-2.15,12.83-2.29,14A.42.42,0,0,0,.84,54l35.64,7.39a1.49,1.49,0,0,0,1.77-1.69L37.1,52.46a72.35,72.35,0,0,0,8.37-3A15.47,15.47,0,0,0,54.71,32.71ZM5.94,40.15A124.79,124.79,0,0,1,8.77,27.73h0A18.37,18.37,0,0,1,22.09,14.29,7,7,0,0,1,31,21.09a6.84,6.84,0,0,1-.11,1.21L4.56,48.18C4.9,46.1,5.32,43.65,5.94,40.15ZM34,57.66s-11.91-2.48-12-2.48h.13A74.28,74.28,0,0,0,34,53.28l.61,3.84A.47.47,0,0,1,34,57.66ZM44,46.57a72.9,72.9,0,0,1-28.31,5.72,76,76,0,0,1-10.09-.76L32.73,25A12.3,12.3,0,0,1,43,23.49a12,12,0,0,1,8.29,8.44A12.26,12.26,0,0,1,44,46.57Z"
    />
  </symbol>
  <g>
    <use xlink:href="#pathSymbol"
         id="path1"></use>
    <use xlink:href="#pathSymbol"
         id="path2"></use>
  </g>

</svg>
<script type="text/javascript">
  function draw(className) {
    var path = document.querySelector(className);
    console.log('path',path)
    var length = path.getTotalLength();
    console.log('length',length);
    // 412
  }

  //draw(".path"); //得到Path的长度为988

</script>
</body>
</html>

```