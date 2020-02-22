#### 使用3DLUT滤镜处理图像
>index.html

```html
<body>
    <canvas id="myCanvas" width="640" height="480" style="border:1px solid #d3d3d3;">
        Your browser does not support the HTML5 canvas tag.</canvas>
    <canvas id="myCanvasLut" width="640" height="480" style="border:1px solid #d3d3d3;">
        Your browser does not support the HTML5 canvas tag.</canvas>
</body>
```
>index.js

```javascript
var srcLutbyte= "data:image/png;base64,xxx"
var img = new Image,
    canvas = document.getElementById("myCanvas"),
    ctx = canvas.getContext("2d"),
    src = "http://i.imgur.com/0vcCTK9.jpg?1"; // insert image url here

img.crossOrigin = "Anonymous";


img.onload = function() {
    canvas.width = img.width;
    canvas.height = img.height;
    ctx.drawImage( img, 0, 0 );
  //  alternateImage(img, canvas, ctx);
    localStorage.setItem( "savedImageData", canvas.toDataURL("image/png") );
    loadNew();
}

img.src = src;
// make sure the load event fires for cached images too
if ( img.complete || img.complete === undefined ) {
    img.src = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///ywAAAAAAQABAAACAUwAOw==";
    img.src = src;
}

var imgLut = new Image,
    canvasLut = document.getElementById("myCanvasLut"),
    ctxLut = canvasLut.getContext("2d"),
    srcLut = "https://gm1.ggpht.com/nX2ZUYrMwPSXu5zGeeoMKyZP_R4nE205ivAdc3_yaccMEy8QYInfY_ynUB-NmrjvPKn0i1k7bdtHyk3Ul99ndvjvoCASud8FdIdq1fRrqDOCGK01rXgZZQ34ATKvtrkoysUCBmTUG70ZW_-TQxHExbu8gjhH-haIg0EuiWgJSxkL45jE1B4xWaOQNQXgJtmb7i1bSVcRgdmJq0XbttjsZnZn3YTW_LYw_3F-WyEEryTRritkZm6CW6NgaoVUfGH6XIaHp5Igs_dzm01lci9XwvoUwS0KA85w3npkjseL0zZX6u-pYWbSXOzkTLDJDMKPpOPt1VH6UUwARlD1YH1dQb0qdq4FrN8_beghJc00UaO9WHgyLyQ-NXMXFt5zXpeKuWtGwWtB0bzDhEvUXUhoDeOwaTbHlEjv3NgrqfzzpLBfLMM9J2BZLZodaEFA6WiroIsq70Qh6g_yMCVg02oi3s-L_2SSW2duayIIcfljyOxmC5AHbjzS2i-4RnKlVzK5Ge39wmiXX_4wtL0nb5XeDPwGbqqJsCPaeIYFN7z43HW6bA--5E3pUo3mjxLPTMSa8T-omZIw7w=w1896-h835-l75"; 
function loadNew(){

imgLut.crossOrigin = "Anonymous2";
imgLut.onload = function() {
    canvasLut.width = imgLut.width;
    canvasLut.height = imgLut.height;
    ctxLut.drawImage( imgLut, 0, 0 );
    filterImage(img, imgLut, canvasLut, ctxLut);
    localStorage.setItem( "savedImageDataLut", canvasLut.toDataURL("image/png") );
}

imgLut.src = srcLutbyte;
// make sure the load event fires for cached images too
if ( imgLut.complete || imgLut.complete === undefined ) {
    imgLut.src = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///ywAAAAAAQABAAACAUwAOw==";
    imgLut.src = srcLutbyte;
}
}
function alternateImage(img, canvastoapply, ctxToApply){
  //var c=document.getElementById("myCanvas");
  //var img=document.getElementById("scream");
  ctxToApply.drawImage(img,0,0);
  var imgData=ctxToApply.getImageData(0,0,canvastoapply.width,canvastoapply.height);
  // invert colors
  for (var i=0;i<imgData.data.length;i+=4){
    imgData.data[i]=255-imgData.data[i];
    imgData.data[i+1]=255-imgData.data[i+1];
    imgData.data[i+2]=255-imgData.data[i+2];
    imgData.data[i+3]=255;
    }
  ctxToApply.putImageData(imgData,0,0);
};

function filterImage(img, filter, canvastoapply, ctxToApply){
  //var c=document.getElementById("myCanvas");
  //var img=document.getElementById("scream");

 //   ctxToApply.drawImage(img,0,0);
  var lutWidth = canvasLut.width;
  var imgData=ctx.getImageData(0,0,canvas.width,canvas.height);
  var filterData=ctxToApply.getImageData(0,0,canvastoapply.width,canvastoapply.height);
    
  // invert colors
  for (var i=0;i<imgData.data.length;i+=4){
    var r=Math.floor(imgData.data[i]/4);
    var g=Math.floor(imgData.data[i+1]/4);
    var b=Math.floor(imgData.data[i+2]/4);    
    var a=255;

            
    var lutX = (b % 8) * 64 + r;
    var lutY = Math.floor(b / 8) * 64 + g;
    var lutIndex = (lutY * lutWidth + lutX)*4;

    var Rr =  filterData.data[lutIndex];
    var Gg =  filterData.data[lutIndex+1];    
    var Bb =  filterData.data[lutIndex+2];
      
    imgData.data[i] = filterData.data[lutIndex];
    imgData.data[i+1] = filterData.data[lutIndex+1];
    imgData.data[i+2] = filterData.data[lutIndex+2];
    imgData.data[i+3] = 255;

  }
  ctx.putImageData(imgData,0,0);
};
```