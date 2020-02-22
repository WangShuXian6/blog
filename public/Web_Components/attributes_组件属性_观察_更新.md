### attributes 组件属性 观察/更新
>HTML中的元素具有属性; 这些是配置元素或以各种方式调整其行为以满足用户所需条件的其他值。

>使用以下属性创建一个组件UserCard：username，address和is-admin（布尔值告诉我们用户是否为admin）。 

>观察这些属性以进行更改并相应地更新组件。

***
>定义属性

```html
<user-card username="Ayush" address="Indore, India" is-admin></user-card>
```

>使用JavaScript中的DOM API来使用getAttribute（attrName）和setAttribute（attrName，newVal）方法来获取和设置属性。

```ts
let myUserCard = document.querySelector('user-card')

myUserCard.getAttribute('username') // Ayush

myUserCard.setAttribute('username', 'Ayush Gupta') 
myUserCard.getAttribute('username') // Ayush Gupta
```
***
#### 观察属性更改
>自定义元素规范v1定义了一种观察属性更改并对这些更改采取操作的简便方法。 在创建我们的组件时，我们需要定义两件事：

>观察到的属性：要在属性更改时得到通知，必须在初始化元素时定义观察到的属性列表，方法是在返回属性名称数组的元素类上放置一个静态的observeAttributes getter。

>attributeChangedCallback（attributeName，oldValue，newValue，namespace）：在元素上更改，追加，删除或替换属性时调用的生命周期方法。 它仅用于观察属性。

#### 创建UserCard组件
>构建UserCard组件，它将使用属性进行初始化，并且我们的组件将观察对其属性所做的任何更改。

>在项目目录中创建index.html文件。 

>还可以使用以下文件创建UserCard目录：UserCard.html，UserCard.css和UserCard.js。

>UserCard.js

```ts
(async () => {
  const res = await fetch('/UserCard/UserCard.html');
  const textTemplate = await res.text();
  const HTMLTemplate = new DOMParser().parseFromString(textTemplate, 'text/html')
                           .querySelector('template');

  class UserCard extends HTMLElement {
    constructor() { ... }
    connectedCallback() { ... }
    
    // Getter to let component know what attributes
    // to watch for mutation
    static get observedAttributes() {
      return ['username', 'address', 'is-admin']; 
    }

    attributeChangedCallback(attr, oldValue, newValue) {
      console.log(`${attr} was changed from ${oldValue} to ${newValue}!`)
    }
  }

  customElements.define('user-card', UserCard);
})();
```

***
#### 使用属性初始化
>创建组件时，我们将为它提供一些初始值，它将用于初始化组件。 

```html
<user-card username="Ayush" address="Indore, India" is-admin="true"></user-card>
```

>在connectedCallback中，我们将使用这些属性并定义与每个属性相对应的变量。

```ts
connectedCallback() {
  const shadowRoot = this.attachShadow({ mode: 'open' });
  const instance = HTMLTemplate.content.cloneNode(true);
  shadowRoot.appendChild(instance);

  // You can also put checks to see if attr is present or not
  // and throw errors to make some attributes mandatory
  // Also default values for these variables can be defined here
  this.username = this.getAttribute('username');
  this.address = this.getAttribute('address');
  this.isAdmin = this.getAttribute('is-admin');
}

// Define setters to update the DOM whenever these values are set
set username(value) {
  this._username = value;
  if (this.shadowRoot)
    this.shadowRoot.querySelector('#card__username').innerHTML = value;
}

get username() {
  return this._username;
}

set address(value) {
  this._address = value;
  if (this.shadowRoot)
    this.shadowRoot.querySelector('#card__address').innerHTML = value;
}

get address() {
  return this._address;
}

set isAdmin(value) {
  this._isAdmin = value;
  if (this.shadowRoot)
    this.shadowRoot.querySelector('#card__admin-flag').style.display = value == true ? "block" : "none";
}

get isAdmin() {
  return this._isAdmin;
}
```

***
#### 观察属性更改
>更改观察到的属性时，将调用attributeChangedCallback。 所以我们需要定义当这些属性发生变化时会发生什么。 重写函数以包含以下内容：

```ts
attributeChangedCallback(attr, oldVal, newVal) {
  const attribute = attr.toLowerCase()
  console.log(newVal)
  if (attribute === 'username') {
    this.username = newVal != '' ? newVal : "Not Provided!"
  } else if (attribute === 'address') {
    this.address = newVal !== '' ? newVal : "Not Provided!"
  } else if (attribute === 'is-admin') {
    this.isAdmin = newVal == 'true';
  }
}
```
***
#### 创建组件
```html
<template id="user-card-template">
  <h3 id="card__username"></h3>
  <p id="card__address"></p>
  <p id="card__admin-flag">I'm an admin</p>
</template>
```
***
#### 使用组件
>使用2个输入元素和一个复选框创建index.html文件，并为所有这些元素定义onchange方法以更新组件的属性。 一旦属性更新，更改也将反映在DOM中。

```html
<html>

<head>
  <title>Web Component</title>
</head>

<body>
  <input type="text" onchange="updateName(this)" placeholder="Name">
  <input type="text" onchange="updateAddress(this)" placeholder="Address">
  <input type="checkbox" onchange="toggleAdminStatus(this)" placeholder="Name">
  <user-card username="Ayush" address="Indore, India" is-admin></user-card>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/webcomponentsjs/1.0.14/webcomponents-hi.js"></script>
  <script src="/UserCard/UserCard.js"></script>
  <script>
    function updateAddress(elem) {
      document.querySelector('user-card').setAttribute('address', elem.value);
    }

    function updateName(elem) {
      document.querySelector('user-card').setAttribute('username', elem.value);
    }

    function toggleAdminStatus(elem) {
      document.querySelector('user-card').setAttribute('is-admin', elem.checked);
    }
  </script>
</body>

</html>
```
***
#### 何时使用属性
>在上一篇文章中，我们为子组件创建了一个API，以便父组件可以使用此API初始化并与它们交互。在这种情况下，如果我们已经有一些配置，希望直接提供而不使用父/其他函数调用，将无法做到。

>使用属性，我们可以非常轻松地提供初始配置。然后可以在构造函数或connectedCallback中提取此配置以初始化组件。

>更改属性以与组件交互可能会有点单调乏味。假设您要将大量json数据传递给组件。这样做需要将json表示为字符串属性，并在组件使用时进行解析。

#### 我们有3种方法可以创建交互式Web组件：

>仅使用属性：这是我们在本文中看到的方法。我们使用属性来初始化组件以及与外部世界进行交互。

>仅使用已创建的函数：这​​是我们在本系列的第2部分中看到的方法，我们使用我们为它们创建的函数初始化并与组件交互。

>使用混合方法：应该使用IMO。在这种方法中，我们使用属性初始化组件，并且对于所有后续交互，只需使用对其API的调用。
