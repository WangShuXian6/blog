#### Web Components modal 模态弹窗

![peek](https://user-images.githubusercontent.com/30850497/61198109-ef6e3700-a70a-11e9-87b7-af6dc669c19a.gif)
##### 定义模态组件

>modal.js

```ts
class Modal extends HTMLElement {
    constructor() {
        super();
        this._modalVisible = false;
        this._modal;
        this.attachShadow({ mode: 'open' });
        this.shadowRoot.innerHTML = `
        <style>
            /* The Modal (background) */
            .modal {
                display: none; 
                position: fixed; 
                z-index: 1; 
                padding-top: 100px; 
                left: 0;
                top: 0;
                width: 100%; 
                height: 100%; 
                overflow: auto; 
                background-color: rgba(0,0,0,0.4); 
            }

            /* Modal Content */
            .modal-content {
                position: relative;
                background-color: #fefefe;
                margin: auto;
                padding: 0;
                border: 1px solid #888;
                width: 80%;
                box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.19);
                -webkit-animation-name: animatetop;
                -webkit-animation-duration: 0.4s;
                animation-name: animatetop;
                animation-duration: 0.4s
            }

            /* Add Animation */
            @-webkit-keyframes animatetop {
                from {top:-300px; opacity:0} 
                to {top:0; opacity:1}
            }

            @keyframes animatetop {
                from {top:-300px; opacity:0}
                to {top:0; opacity:1}
            }

            /* The Close Button */
            .close {
                color: white;
                float: right;
                font-size: 28px;
                font-weight: bold;
            }

            .close:hover,
            .close:focus {
            color: #000;
            text-decoration: none;
            cursor: pointer;
            }

            .modal-header {
            padding: 2px 16px;
            background-color: #000066;
            color: white;
            }

            .modal-body {padding: 2px 16px; margin: 20px 2px}

        </style>
        <button>Open Modal</button>
        <div class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <span class="close">&times;</span>
                    <slot name="header"><h1>Default text</h1></slot>
                </div>
                <div class="modal-body">
                    <slot><slot>
                </div>
            </div>
        </div>
        `
    }
    connectedCallback() {
        this._modal = this.shadowRoot.querySelector(".modal");
        this.shadowRoot.querySelector("button").addEventListener('click', this._showModal.bind(this));
        this.shadowRoot.querySelector(".close").addEventListener('click', this._hideModal.bind(this));
    }
    disconnectedCallback() {
        this.shadowRoot.querySelector("button").removeEventListener('click', this._showModal);
        this.shadowRoot.querySelector(".close").removeEventListener('click', this._hideModal);
    }
    _showModal() {
        this._modalVisible = true;
        this._modal.style.display = 'block';
    }
    _hideModal() {
        this._modalVisible = false;
        this._modal.style.display = 'none';
    }
}
customElements.define('pp-modal',Modal);
```
***
##### 使用 模态组件

>index.html

```html
<!DOCTYPE html>
<html>

<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
 
  <script src="./modal.js"></script>
</head>

<body>

  <h2>Modal web component with vanilla JS.</h2>

  <pp-modal>
    <h1 slot="header">Information Box</h1>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna
    aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
  </pp-modal>

</body>

</html>
```