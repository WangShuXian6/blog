#### CSS自定义属性（--themeColor）
>https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties

#### 基本用法

##### 声明一个局部变量：

```css
element {
  --main-bg-color: brown;
}
```

##### 使用一个局部变量：

```css
element {
  background-color: var(--main-bg-color);
}
```
***
##### 每个组件可以单独设置（并不推荐，影响统一风格），也可以全局设置（推荐）。

```css
xy-button{
    --themeColor: #42b983;/**单独设置**/
}
:root {
    --themeColor: #42b983;/**全局设置**/
}
```

##### 也可以通过JavaScript实时修改。

```ts
document.body.style.setProperty('--themeColor','#F44336');
```