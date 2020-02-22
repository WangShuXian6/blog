#### rgb to cmyk 
```ts
function RgbToCmyk(R,G,B)
{
 if ((R == 0) && (G == 0) && (B == 0)) {
 return [0, 0, 0, 1];
 } else {
 let calcR = 1 - (R/255),
 calcG = 1 - (G/255),
 calcB = 1 - (B/255);
 let K = Math.min(calcR, Math.min(calcG, calcB)),
 C = (calcR - K)/(1 - K),
 M = (calcG - K)/(1 - K),
 Y = (calcB - K)/(1 - K);
 return [C, M, Y, K];
 }
}
```

```ts
function get_percentage(value, model){
 if (model =="CMYK") return value;//CMYK values are 0-100 (if 0-1 just * 100)
 if (model =="RGB" ) return (value/255)* 100;
}
//a call with the value of 66 in RGB model:
document.write( get_percentage( 66,"RGB"));
```