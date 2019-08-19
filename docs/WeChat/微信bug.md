#### 微信 bug

#### 禁止微信下拉
```ts
document.addEventListener('touchmove', function (event) {
        event.preventDefault();
      });
```

***
#### 视频自动播放 
>需要轻点事件 tab, click

```html
<video class="video" preload="auto" :class="{hide:showTip || !showVideo}"
             webkit-playsinline="true"
             playsinline="true"
             x5-playsinline=""
             x-webkit-airplay="allow"
             :poster="posterSrc"
             ref="video"
             controls
      >
```

```ts
this.videEl = this.$refs.video;
this.videEl.play();
```
