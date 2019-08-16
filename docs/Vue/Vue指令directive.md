#### Vue 指令  directive

##### drag

>drag.ts

```ts
export default {
  install(Vue, options) {
    Vue.directive('drag', {
      bind(el, binding, vnode, oldVnode) {
        let startX
        let startY
        let translateX = 0
        let translateY = 0
        let translateXLast = 0
        let translateYLast = 0
        let distance = 0

        function touchStart(event) {
          event.preventDefault()
          if (!event.touches.length) return;
          el.classList.add('dragable')
          el.classList.add('ease')
          let touch = event.touches[0]
          startX = touch.pageX;
          startY = touch.pageY;
        }

        function touchMove(event) {
          event.preventDefault();
          if (!event.touches.length) return;

          let touch = event.touches[0]
          let x = touch.pageX - startX
          let y = touch.pageY - startY;

          translateX = x
          translateY = y

          el.style.transform = `translate(${translateXLast + x}px, ${translateYLast + y}px)`
        }

        function touchEnd(event) {
          translateXLast += translateX
          translateYLast += translateY
          endAnimation()
        }

        function endAnimation() {
          el.style.transform = `translate(${translateXLast}px, ${translateYLast - distance}px`
          el.style.opacity = 0
          setTimeout(() => {
            reset()
          }, 500)
        }

        function reset() {
          translateX = 0
          translateY = 0
          translateXLast = 0
          translateYLast = 0
          el.classList.remove('ease')
          el.classList.add('dragable')
          el.style.transform = `translate(${0}px, ${0}px`
          el.style.opacity = 1
        }

        function init() {
          distance = binding.value
          el.addEventListener('touchstart', touchStart, false)
          el.addEventListener('touchmove', touchMove, false)
          el.addEventListener('touchend', touchEnd, false)
        }

        init()
      }
    })
  }
}
```

>drag.less

```less

.dragable {
  z-index: 2;
  display: flex;
}

.dragable.fix {
  z-index: 1;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.ease {
  transition: all 0.3s linear;
}

```

>use
>main.ts
```ts
import Drag from './directive/drag'

Vue.use(Drag)
```

>app.vue

```vue
<template>
   <div class="content-inner" v-drag="goods.distance">       
    </div>
</template>
```