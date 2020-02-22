#### typescript 内置对象

#### canvasEl: HTMLCanvasElement

#### e:MouseEvent

```ts
const canvasEl=
  <HTMLCanvasElement> document.getElementById('main-canvas') ||
  <HTMLCanvasElement> document.createElement('canvas')
```