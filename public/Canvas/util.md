#### input 获取图片后 画在 canvas 上
```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="file" name="image" id="file" accept="image/*" capture/>
<canvas id="canvas" width="300" height="400"></canvas>
<script>

    const imageInputWrapper = document.querySelector('#file')
    const initCanvas = document.querySelector('#canvas')
    const initCanvasCtx = initCanvas.getContext('2d')

    // const imageFileToBase64=(imageFile,callback)=>{
    //     let reader = new FileReader()
    //     reader.readAsDataURL(imageFile)
    //     reader.onload=(e)=>{
    //         const base64=e.target.result
    //         callback(base64)
    //     }
    // }
  
    // const loadImage=(imageSrc,callback)=>{
    //     let image = new Image()
    //     image.src = imageSrc
    //     image.onload=()=>{
    //         callback(image)
    //     }
    // }
   
    const drawWhiteCanvas=(initCanvasCtx)=>{
        initCanvasCtx.fillStyle='red'
        initCanvasCtx.fillRect(0, 0, 900,900)
    }

    // const drawImage=(initCanvasCtx,image)=>{
    //     initCanvasCtx,drawImage(image,0,0)
    // }

    imageInputWrapper.addEventListener('change', () => {
        console.log('imageInputWrapper--', imageInputWrapper.files[0])
        const imageFile = imageInputWrapper.files[0]
        drawWhiteCanvas(initCanvasCtx)

        let reader = new FileReader()
        reader.readAsDataURL(imageFile)
        reader.onload = (e) => {
            const base64 = e.target.result
            let image = new Image()
            image.src = base64
            image.onload = () => {
                console.log('image--', image)
                initCanvasCtx.drawImage(image, 0, 0, 200, 200);
            }
        }
    })

</script>
</body>
</html>

```

#### 选取图片后 再 经 canvas 处理后 最后由 PhotoClip 处理导出
```html
<!doctype html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, minimal-ui"/>
    <title>PhotoClip</title>
    <style>
        body {
            margin: 0;
            text-align: center;
        }

        #clipArea {
            height: 300px;
        }

        #file,
        #clipBtn {
            margin: 20px;
        }

        #view {
            margin: 0 auto;
            width: 200px;
            height: 200px;
            background-color: #666;
        }

        #init-canvas{
            position: fixed;
            left: -100vw;
            bottom: -100vh;
            z-index: -100;
            opacity: 0;
        }
    </style>
</head>
<body ontouchstart="">

<div id="clipArea"></div>
<!--<input type="file" id="file">-->
<input type="file" name="image" id="file" accept="image/*" capture/>

<button id="clipBtn">截取</button>
<div id="view"></div>

<canvas id="init-canvas" width="500" height="500"></canvas>

<script src="js/hammer.min.js"></script>
<script src="js/iscroll-zoom-min.js"></script>
<script src="js/lrz.all.bundle.js"></script>

<script src="js/PhotoClip.js"></script>

<script>
    const imageInputWrapper = document.querySelector('#file')
    const initCanvas = document.querySelector('#init-canvas')
    const initCanvasCtx = initCanvas.getContext('2d')

    const drawWhiteCanvas = (initCanvasCtx) => {
        initCanvasCtx.fillStyle = 'red'
        initCanvasCtx.fillRect(0, 0, 900, 900)
    }

    imageInputWrapper.addEventListener('change', () => {
        console.log('imageInputWrapper--', imageInputWrapper.files[0])
        const imageFile = imageInputWrapper.files[0]
        drawWhiteCanvas(initCanvasCtx)

        let reader = new FileReader()
        reader.readAsDataURL(imageFile)
        reader.onload = (e) => {
            const base64 = e.target.result
            let image = new Image()
            image.src = base64
            image.onload = () => {
                const canvasWidth = initCanvas.width
                const canvasHeight = initCanvas.height
                const imageWidth = image.width
                const imageHeight = image.height

                let drawX
                let drawY
                let drawWidth
                let drawHeight

                if (imageWidth >= imageHeight) {
                    drawX = 0
                    drawY = (canvasHeight - imageHeight / imageWidth * canvasWidth) / 2
                    drawWidth = canvasWidth
                    drawHeight = imageHeight / imageWidth * canvasWidth
                } else {
                    drawX = (canvasWidth - imageWidth / imageHeight * canvasHeight) / 2
                    drawY = 0
                    drawWidth = imageWidth / imageHeight * canvasHeight
                    drawHeight = canvasHeight
                }

                initCanvasCtx.drawImage(image, drawX, drawY, drawWidth, drawHeight);

                const finalImage = initCanvas.toDataURL("image/jpeg", 1.0)
                initClip(finalImage)
            }
        }
    })

    const initClip = (img) => {
        let pc = new PhotoClip('#clipArea', {
            size: [260, 260],
            outputSize: 640,
            //adaptive: ['60%', '80%'],
            //file: '#file',
            img,
            view: '#view',
            ok: '#clipBtn',
            //img: 'img/mm.jpg',
            loadStart: function () {
                console.log('开始读取照片');
            },
            loadComplete: function () {
                console.log('照片读取完成');
            },
            done: function (dataURL) {
                console.log(dataURL);
            },
            fail: function (msg) {
                alert(msg);
            }
        });

        // 加载的图片必须要与本程序同源，否则无法截图
        pc.load(img);
    }
</script>
</body>
</html>


```

#### 图片始终显示完整的最长边
```javascript
                const canvasWidth = initCanvas.width
                const canvasHeight = initCanvas.height
                const imageWidth = image.width
                const imageHeight = image.height

                let drawX
                let drawY
                let drawWidth
                let drawHeight

                if (imageWidth >= imageHeight) {
                    drawX = 0
                    drawY = (canvasHeight - imageHeight / imageWidth * canvasWidth) / 2
                    drawWidth = canvasWidth
                    drawHeight = imageHeight / imageWidth * canvasWidth
                } else {
                    drawX = (canvasWidth - imageWidth / imageHeight * canvasHeight) / 2
                    drawY = 0
                    drawWidth = imageWidth / imageHeight * canvasHeight
                    drawHeight = canvasHeight
                }

                initCanvasCtx.drawImage(image, drawX, drawY, drawWidth, drawHeight);

```

#### 触摸自由移动物体
>index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="./index.less">
    <title>Title</title>
    <meta name="viewport" content="width=device-width,height=device-height, user-scalable=no,initial-scale=1, minimum-scale=1, maximum-scale=1,target-densitydpi=device-dpi ">  <meta name="apple-mobile-web-app-capable" content="yes" />
    <meta name="apple-mobile-web-app-status-bar-style" content="black" />
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
</head>
<body>
<canvas width="500" height="1160" id="main-canvas"></canvas>

<script src="./index.ts"></script>
</body>
</html>

```

>index.js

```ts
const canvasEl =
    document.getElementById('main-canvas')
const ctx = canvasEl.getContext('2d')

const loadImage = (fn) => {
    const img = new Image();   // 创建img元素
    img.onload = () => {
        fn(img)
    }
    img.onerror = (error) => {
        console.error('error', error)
    }
    img.src = 'https://xxx.xxx.com/wsx/slideshow/horizontal/google-steve-irwins-57th/a_1.png'
}
//
// loadImage((img) => {
//     ctx.save()
//     ctx.translate(414, 0)
//     ctx.rotate(90 * Math.PI / 180)
//     ctx.drawImage(img, 0, 0, 1160, 500, 0, 0, 1160, 500)
//     ctx.restore()
// })

var previousTouchX
var previousTouchY
var step = 0
var img


const touchmove = (e) => {
    const touchX = e.changedTouches[0].clientX
    const touchY = e.changedTouches[0].clientY
    console.log('touchX', touchX)
    step += (touchX - previousTouchX)
    console.log('step', step)

    if (previousTouchX) {
        console.log('draw')
        ctx.clearRect(0, 0, 1160, 500)

        ctx.drawImage(img, 0, 0, 1160, 500, step, 0, 1160, 500)
    }

    previousTouchX = touchX
    previousTouchY = touchY
}

const touchstart = (e) => {
    previousTouchX = e.changedTouches[0].clientX
    previousTouchY = e.changedTouches[0].clientY
    console.log('start-touchX', previousTouchX)
}

loadImage((imgData) => {
    console.log('loaded')
    img = imgData
    ctx.drawImage(img, 0, 0, 1160, 500, step, 0, 1160, 500)
    canvasEl.addEventListener('touchstart', touchstart.bind(this), false)
    canvasEl.addEventListener('touchmove', touchmove.bind(this), false)
})



```

#### canvas 点击判断
```ts
import {ArticlesCanvasInfo} from "@/config/canvas";

type ImageData = {
  data: Uint8ClampedArray,
  width: number,
  height: number
}

const calcDrawPos = (image: HTMLImageElement, canvasEl: HTMLCanvasElement) => {
  const imageWidth = image.width;
  const imageHeight = image.height;
  const canvasWidth = canvasEl.width;
  const canvasHeight = canvasEl.height;
  const imageRatio = imageWidth / imageHeight;
  const canvasRatio = canvasWidth / canvasHeight;

  if (imageRatio >= canvasRatio) {
    const scale = imageWidth / canvasWidth;
    const drawHeight = imageHeight / scale;
    const drawY = (canvasHeight - drawHeight) / 2;
    return {
      x: 0,
      y: drawY,
      width: canvasWidth,
      height: drawHeight
    };
  } else {
    const scale = imageHeight / canvasHeight;
    const drawWidth = imageWidth / scale;
    const drawX = (canvasWidth - drawWidth) / 2;
    return {
      x: drawX,
      y: 0,
      width: drawWidth,
      height: canvasHeight
    };
  }
};

export const drawToCanvas = (imgData, canvasEl?: HTMLCanvasElement): Promise<boolean> | boolean => {
  let currrentCanvasEl
  if (!canvasEl) {
    currrentCanvasEl = document.createElement('canvas')
  } else {
    currrentCanvasEl = canvasEl
  }

  if (!currrentCanvasEl) return false

  currrentCanvasEl.width = ArticlesCanvasInfo.width
  currrentCanvasEl.height = ArticlesCanvasInfo.height

  const ctx = currrentCanvasEl.getContext('2d');
  if (!ctx) {
    return false;
  }
  return new Promise((resolve) => {
    const img = new Image;
    img.src = imgData;
    img.crossOrigin = 'Anonymous';
    img.setAttribute('crossOrigin', 'Anonymous');
    img.onload = () => {
      //ctx.fillStyle = '#FFF';
      //ctx.fillRect(0, 0, ArticlesCanvasInfo.width, ArticlesCanvasInfo.height);
      const drawInfo = calcDrawPos(img, currrentCanvasEl);
      ctx.drawImage(img, 0, 0, img.width, img.height, drawInfo.x, drawInfo.y, drawInfo.width, drawInfo.height);
      // const strDataURI = currrentCanvasEl.toDataURL('image/jpeg', 0.5);
      resolve(true)
    };
  });
};

export const getDataURI = (imgData, canvasEl?: HTMLCanvasElement) => {
  let currrentCanvasEl
  if (!canvasEl) {
    currrentCanvasEl = document.createElement('canvas')
  } else {
    currrentCanvasEl = canvasEl
  }

  if (!currrentCanvasEl) return false

  currrentCanvasEl.width = ArticlesCanvasInfo.width
  currrentCanvasEl.height = ArticlesCanvasInfo.height

  const ctx = currrentCanvasEl.getContext('2d');
  if (!ctx) {
    return false;
  }
  return new Promise((resolve) => {
    const img = new Image;
    img.src = imgData;
    img.crossOrigin = 'Anonymous';
    img.setAttribute('crossOrigin', 'Anonymous');
    img.onload = () => {
      //ctx.fillStyle = '#FFF';
      //ctx.fillRect(0, 0, ArticlesCanvasInfo.width, ArticlesCanvasInfo.height);
      const drawInfo = calcDrawPos(img, currrentCanvasEl);
      ctx.drawImage(img, 0, 0, img.width, img.height, drawInfo.x, drawInfo.y, drawInfo.width, drawInfo.height);
      const strDataURI = currrentCanvasEl.toDataURL('image/jpeg', 0.5);
      resolve(strDataURI)
    };
  });
}

export const getCanvas = (imgData, canvasEl?: HTMLCanvasElement): Promise<HTMLCanvasElement> | false => {
  let currrentCanvasEl
  if (!canvasEl) {
    currrentCanvasEl = document.createElement('canvas')
  } else {
    currrentCanvasEl = canvasEl
  }

  if (!currrentCanvasEl) return false

  currrentCanvasEl.width = ArticlesCanvasInfo.width
  currrentCanvasEl.height = ArticlesCanvasInfo.height

  const ctx = currrentCanvasEl.getContext('2d');
  if (!ctx) {
    return false;
  }
  return new Promise((resolve) => {
    const img = new Image;
    img.src = imgData;
    img.crossOrigin = 'Anonymous';
    img.setAttribute('crossOrigin', 'Anonymous');
    img.onload = () => {
      //ctx.fillStyle = '#FFF';
      //ctx.fillRect(0, 0, ArticlesCanvasInfo.width, ArticlesCanvasInfo.height);
      const drawInfo = calcDrawPos(img, currrentCanvasEl);
      ctx.drawImage(img, 0, 0, img.width, img.height, drawInfo.x, drawInfo.y, drawInfo.width, drawInfo.height);
      // const strDataURI = currrentCanvasEl.toDataURL('image/jpeg', 0.5);
      resolve(currrentCanvasEl)
    };
  });
}

export const getImageData = (canvasEl: HTMLCanvasElement): ImageData | false => {
  if (!canvasEl) {
    console.warn('canvasEl 对象错误', canvasEl)
    return false
  }
  const {width, height} = canvasEl
  let ctx = canvasEl.getContext('2d')
  if (!ctx) {
    console.warn('ctx 对象错误', ctx)
    return false
  }
  return ctx.getImageData(0, 0, width, height)
}

export const getPixelSerialNumber = (clickX: number, clickY: number, canvasWidth: number = ArticlesCanvasInfo.width) => {
  return (clickY - 1) * canvasWidth + clickX
}

export const getPixelSerialAlpha = (Uint8ClampedArray: Uint8ClampedArray, pixelSerialNumber: number): number | false => {
  console.log('typeof pixelSerialNumber', typeof pixelSerialNumber)
  if (typeof pixelSerialNumber === 'number') {
    console.log('type number')
    const AlphaIndex = pixelSerialNumber * 4 + 3
    console.log('Uint8ClampedArray[AlphaIndex]', Uint8ClampedArray[AlphaIndex])
    return Uint8ClampedArray[AlphaIndex]
  } else {
    console.warn('pixelSerialNumber 不是数字:', pixelSerialNumber)
    return false
  }
}

// 要求，clickCanvasDom 与 checkedImage 宽高比相等，或者 以显示完整图片的策略在 canvas 上绘图
export const isClickImage = (clickCanvasDom: HTMLCanvasElement, checkedImage, e: MouseEvent): Promise<boolean> => {
  return new Promise(async (resolve) => {
    let x = e.pageX - clickCanvasDom.getBoundingClientRect().left
    let y = e.pageY - clickCanvasDom.getBoundingClientRect().top
    const pixelSerialNumber = getPixelSerialNumber(x, y)

    let checkedCanvasDom = await getCanvas(checkedImage)

    if (checkedCanvasDom instanceof HTMLCanvasElement) {
      let imageData = getImageData(checkedCanvasDom)
      if (!imageData) {
        resolve(false)
        return false
      } else {
        const pixelSerialAlpha = getPixelSerialAlpha(imageData.data, pixelSerialNumber)
        if (!pixelSerialAlpha)
          console.log('pixelSerialAlpha', pixelSerialAlpha)
        if (pixelSerialAlpha !== 0) {
          resolve(true)
          console.log('点击了图形')
        } else {
          resolve(false)
          console.log('点击了空白')
        }
      }
    } else {
      console.warn('canvasDom 不是 canvas 类型')
      resolve(false)
    }
  })

}

```

#### svg 字符串转为 svg url
```ts
export const generateSvg = (svgString: string) => {
  let svg64 = btoa(svgString);
  var b64Start = 'data:image/svg+xml;base64,';
  let url= b64Start + svg64;
  return url
}
```