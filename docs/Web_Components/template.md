#### template
```ts
(function () {
    class MidociLayOut extends HTMLElement {
      static get observedAttributes() {
        return ['acitve-title', 'active-sub-title']
      }
      
      constructor() {
        super()
        this.attachShadow({mode: 'open'})
        this.shadowRoot.innerHTML = `
          <style>
          
          </style>
          
          <div class="wrapper">
          
         
          </div>
        `
  
        this._a = ''
      }
  
      connectedCallback() {
      }
  
      disconnectedCallback() {
  
      }
  
      attributeChangedCallback(attr, oldVal, newVal) {
        // const attribute = attr.toLowerCase()
        // if (attribute === 'descriptions') {
        //   console.log(1)
        //   this.render(newVal)
        // }
      }
  
    }
  
    const FrozenMidociLayOut = Object.freeze(MidociLayOut);
    customElements.define('midoci-lay-out', FrozenMidociLayOut);
  })()
  
```