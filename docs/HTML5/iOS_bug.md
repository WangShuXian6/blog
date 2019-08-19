#### iOS bug

>os12，输入框输入后后点击不了确定

```html
<input type="text" @blur="blur_pop" placeholder="">
```

```ts
blur_pop:function () {
         document.body.scrollTop = 0;
},
```

***
```ts
document.querySelectorAll('input').forEach((input)=>{
        console.log(input)
        input.addEventListener('blur',()=>{
          document.body.scrollTop = 0;
        },true)
      })
```