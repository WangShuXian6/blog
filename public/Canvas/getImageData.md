#### CanvasRenderingContext2D.getImageData()

>返回一个ImageData对象，用来描述canvas区域隐含的像素数据，这个区域通过矩形表示，起始点为(sx, sy)、宽为sw、高为sh

```bash
ImageData ctx.getImageData(sx, sy, sw, sh)
```

#### 参数
>sx 将要被提取的图像数据矩形区域的左上角 x 坐标。

>sy 将要被提取的图像数据矩形区域的左上角 y 坐标。

>sw 将要被提取的图像数据矩形区域的宽度。

>sh 将要被提取的图像数据矩形区域的高度。

#### 返回值 一个ImageData 对象，包含canvas给定的矩形图像数据

#### 错误抛出 IndexSizeError
>如果高度或者宽度变量为0，则抛出错误

***

#### getImageData()
```html
<canvas id="canvas"></canvas>
```
```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
ctx.rect(10, 10, 100, 100);
ctx.fill();

console.log(ctx.getImageData(50, 50, 100, 100));
// ImageData { width: 100, height: 100, data: Uint8ClampedArray[40000] }
```
>此画布的四个角落分别表示为(left, top), (left + width, top), (left, top + height), 以及(left + width, top + height)四个点。这些坐标点被设定为画布坐标空间元素。

>任何在画布以外的元素都会被返回成一个透明黑的ImageData对像


***

#### ImageData 对象
>https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas

>canvas对象真实的像素数据

>只读属性

>width 图片宽度，单位是像素

>height 图片高度，单位是像素

>data  Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）。

>data属性返回一个 Uint8ClampedArray，它可以被使用作为查看初始像素数据。每个像素用4个1bytes值(按照红，绿，蓝和透明值的顺序; 这就是"RGBA"格式) 来代表。每个颜色值部份用0至255来代表。每个部份被分配到一个在数组内连续的索引，左上角像素的红色部份在数组的索引0位置。像素从左到右被处理，然后往下，遍历整个数组。

>Uint8ClampedArray  包含高度 × 宽度 × 4 bytes数据，索引值从0到(高度×宽度×4)-1

>读取图片中位于第50行，第200列的像素的蓝色部份

```javascript
blueComponent = imageData.data[((50*(imageData.width*4)) + (200*4)) + 2]

let numBytes = imageData.data.length
```

***

>创建一个新的具体特定尺寸的ImageData对象。所有像素被预设为透明黑

```javascript
let myImageData = ctx.createImageData(width, height)
```

>创建一个被anotherImageData对象指定的相同像素的ImageData对象。这个新的对象像素全部被预设为透明黑。这个并非复制了图片数据。

```javascript
let myImageData = ctx.createImageData(anotherImageData)
```

***

#### putImageData() 方法对场景进行像素数据的写入
>在场景内左上角绘制myImageData代表的图片

```javascript
ctx.putImageData(myImageData, 0, 0)
```

***

####  保存图片
>HTMLCanvasElement  提供一个toDataURL()方法，此方法在保存图片的时候非常有用。它返回一个包含被类型参数规定的图像表现格式的数据链接。返回的图片分辨率是96dpi。

>canvas.toDataURL('image/png')

>默认设定。创建一个PNG图片。

>https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL

>canvas.toDataURL('image/jpeg', quality)

>创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。

>https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL

>当从画布中生成了一个数据链接，例如，你可以将它用于任何<image>元素，或者将它放在一个有download属性的超链接里用于保存到本地。

>也可以从画布中创建一个Blob对像。

>canvas.toBlob(callback, type, encoderOptions)

>这个创建了一个在画布中的代表图片的Blob对像。

>https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toBlob

***