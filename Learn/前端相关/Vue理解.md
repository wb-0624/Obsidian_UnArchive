相较于以前的前端写法，html写结构，css写样式，js写脚本相比。Vue引入了新的概念--组件。

# Vue实例对象

Vue实例对象是Vue.js中最基本的单元。由`Vue(构造参数)`构造得到。创建得到的实例对象也就是所谓的Vue应用。可以用来注册为组件。

使用`Vue.createApp(构造参数)`来创建一个Vue实例。
```js
const app = Vue.createApp({
  /* 选项 */
})
```

[构造参数有哪些？](Vue文档.md##构造参数)

# Vue组件

![](img/组件的组织.png)

最上层的组件，也被称为根组件，是整个页面最底层的框架。对应白色的部分。

第二层的三个组件，对应三个灰色的部分。

第三层的组件，对应黑色的部分。

可以发现，组件之间的层级关系，对应着页面之间的嵌套关系。

所以从根组件开始，就能延展至整个应用，不只一个页面。

那么我就要把根组件，挂载到DOM中，从而使得浏览器能够渲染出来。


## 组件注册

无非就是把其他的Vue实例，注册到它的父级，使得父级能够直接使用。

```js
const app = Vue.createApp({...})
app.component('my-component-name', {
  /* ... */
})
```

## 组件的使用

组件在父级注册后，父级的实例就可以像html标签一样，调用子组件。

```html
<my-component-name>
    
<my-component-name/>
```


## 根组件挂载

因为其他的组件层级嵌套下，最终都会在根组件下面，所以只要把根组件挂载到DOM中，就相当于整个Vue的应用被挂载到了DOM中。

Vue项目的页面框架是在根路径下的`index.html`中。

```html
<!DOCTYPE html>
<html lang="en" translate="no">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <script>
      if (!!window.ActiveXObject || "ActiveXObject" in window) {
        alert("当前浏览器不支持，请使用360浏览器极速模式或谷歌浏览器等现代浏览器");
      }

      // todo https://github.com/vitejs/vite/issues/2618
      window.global = window;
    </script>
    <script type="text/javascript" src="https://gosspublic.alicdn.com/aliyun-oss-sdk-6.17.1.min.js"></script>
    <title>Vite App</title>
  </head>

  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

可以发现，脚本就在`src/main.js`中。

```js
import {createApp} from "vue"
import App from "./App.vue"
import "./index.css"
import {useWebkit} from "./plugins/webkit"

  

const app = createApp(App)
useWebkit(app)

  

app.config.productionTip = false
app.mount("#app")
```

最后一行就表示，把app挂载到 `id = "app"`的DOM当中。

至此就完成了Vue在页面中的展示，接下去就是往根组件中不断添加其他组件即可。

## 单文件组件

用一个.vue文件来描述一个组件，也可以用于注册组件。

单文件通常由三个部分组成。

-   `<script>` 部分是一个标准的 JavaScript 模块。它应该导出一个 Vue 组件定义作为其默认导出。
-   `<template>` 部分定义了组件的模板。组件的内容。
-   `<style>` 部分定义了与此组件关联的 CSS。组件的样式。


